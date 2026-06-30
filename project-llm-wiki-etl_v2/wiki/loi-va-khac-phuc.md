# Lỗi & Khắc Phục - Hướng Dẫn Debug

**Tóm tắt**: Trang này ghi lại các lỗi thường gặp trong quá trình chạy ETL, kèm theo nguyên nhân gốc rễ, triệu chứng nhận diện, và hướng khắc phục chi tiết.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (PROJECT_CHRONICLE.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. DEADLOCK - Treo Pipeline Tầng 1

### Triệu Chứng
```
[14:30:45] [hanoi] [DimensionLoader] [START] Nạp DMBenhNhan...
[14:30:47] [hanoi] [DimensionLoader] [INFO] TRUNCATE hoàn tất
[14:31:00] ... (chờ 13 giây, không tiến triển)
[15:00:00] Timeout - Dừng tiến trình
```

### Nguyên Nhân Gốc
```
Session A (pyodbc)                     Session B (BCP subprocess)
├─ EXECUTE("TRUNCATE TABLE...")
├─ Giữ Sch-M (schema modification) lock
├─ (chưa commit, giữ lock)
│
└─ BCP IN process
    └─ Chờ lock → vĩnh viễn treo
```

(nguồn: PROJECT_CHRONICLE.md - ADR-14)

### Cách Khắc Phục

**Nguyên Tắc**: `TRUNCATE` phải `commit()` **ngay lập tức** để giải phóng lock

```python
# ❌ SAI - Để lock giữ lâu
connection.execute("TRUNCATE TABLE [stg_nano_v2].[ThuPhi]")
# ... làm việc khác ...
connection.commit()  # Mới commit

# ✅ ĐÚNG - Commit ngay
connection.execute("TRUNCATE TABLE [stg_nano_v2].[ThuPhi]")
connection.commit()  # Giải phóng lock ngay

time.sleep(0.1)  # Chờ DB xử lý lock release
subprocess.run(bcp_command)  # Giờ BCP có thể ghi
```

**Kiểm Tra**:
- Log phải có 2 dòng rõ ràng:
  - `[INFO] TRUNCATE hoàn tất`
  - `[INFO] Bắt đầu BCP IN`
  - Khoảng cách giữa 2 dòng < 1 giây (không chờ lâu)

(nguồn: PROJECT_CHRONICLE.md - ADR-14, ADR-22)

## 2. MemoryError - Nạp Bảng Lớn Bị Văng

### Triệu Chứng
```
[14:30:45] [hanoi] [DimensionLoader] [START] Nạp DMBenhNhan...
[14:30:50] [hanoi] [DimensionLoader] [INFO] Đã nạp 50000 rows
[14:31:00] [hanoi] [DimensionLoader] [ERROR] MemoryError: Unable to allocate 2.5GB
```

### Nguyên Nhân Gốc
```python
# ❌ SAI - Bật fast_executemany với cột MAX
staging_cursor.fast_executemany = True  # Cấp phát buffer theo max size
rows = cursor.fetchmany(10000)  # 10k rows × MAX(NVARCHAR)
staging_cursor.executemany(insert_sql, rows)  # BOOM - 2.5GB RAM needed
```

Với cột `NVARCHAR(MAX)/VARCHAR(MAX)`:
- ODBC ước lượng bộ nhớ theo kích thước **lý thuyết tối đa** (thường 2GB+)
- `fast_executemany` cấp phát buffer trước
- Batch size lớn → RAM yêu cầu > available

(nguồn: PROJECT_CHRONICLE.md - ADR-18, ADR-38)

### Cách Khắc Phục

**Nguyên Tắc**: Tắt `fast_executemany`, hạ batch size

```python
# ✅ ĐÚNG - Tắt fast_executemany, chunk 1000 rows
staging_cursor.fast_executemany = False  # Dùng single-row insert ngầm ODBC

batch_size = 1000  # Nhỏ để kiểm soát RAM
rows = cursor.fetchmany(batch_size)

while rows:
    staging_cursor.executemany(insert_sql, rows)
    ods_connection.commit()  # Commit mỗi batch
    rows = cursor.fetchmany(batch_size)  # Fetch batch tiếp theo
```

**Kết Quả**:
- RAM per batch: ~50-100MB (có thể dự đoán)
- Tốc độ: Chậm hơn ~2-3x nhưng **ổn định 100%**
- Không MemoryError

**Quy Tắc**:
- `fast_executemany` là tính năng **có điều kiện**, không bật mặc định
- Với schema chứa MAX column, luôn tắt
- Nhỏ RAM > tốc độ nhanh (ưu tiên ổn định)

(nguồn: PROJECT_CHRONICLE.md - ADR-18, ADR-38)

## 3. BCP Error 22005 - Cast Type Mismatch

### Triệu Chứng
```
[14:35:20] [hanoi] [FactLoader] [ERROR] BCP copy in failed (22005)
[14:35:20] Error Code: [SQLSTATE 22005] [SQL Server] Invalid character value
for cast specification (Msg 8114)
```

### Nguyên Nhân Gốc

```
File Data (từ BCP OUT):
├─ Col1: "100"         (INT)
├─ Col2: ""            (Empty string cho INT)
├─ Col3: "2026-05-20"  (DATE)
└─ Lỗi: Empty string không thể cast INT

Schema:
├─ [Col1] INT        ← File gửi "100" OK
├─ [Col2] INT        ← File gửi "" → FAIL
└─ [Col3] DATETIME   ← ...
```

(nguồn: PROJECT_CHRONICLE.md - ADR-15, ADR-23)

### Nguyên Nhân Sâu Hơn

```python
# Cơ chế cũ: Black-list datatype loại cột
exclude_datatypes = ["INT", "DATETIME"]
columns = ["Col1", "Col2", "Col3"]

# Dynamic SELECT (SAI):
SELECT Col1, Col3 FROM Table  # Col2 bị loại!
# Output: 2 cột
# Landing Table: 3 cột
# → BCP IN: Cột 3 nhận giá trị Col3, lệch schema!
```

(nguồn: PROJECT_CHRONICLE.md - ADR-23, ADR-31)

### Cách Khắc Phục

**Phương Pháp 1: Masking NULL (cũ, còn dùng tạm)**
```sql
-- Không loại cột, mask bằng NULL
SELECT
  [Col1],
  CAST(NULL AS NVARCHAR(MAX)) AS [Col2],  -- Mask, giữ vị trí
  [Col3]
FROM [Production].[Table]
```

**Phương Pháp 2: Whitelist Column (Mới, khuyên dùng)**
```yaml
# config/tables.yaml
incremental_tables:
  - name: "ThuPhiDichVu"
    selected_columns:
      - Col1         # Chỉ lấy 2 cột cần thiết
      - Col3
    # Col2 không khai báo = không lấy
```

```sql
-- Dynamic SELECT từ Whitelist (chỉ 2 cột)
SELECT [Col1], [Col3] FROM [Production].[Table]
-- Landing table cũng chỉ 2 cột
-- Schema khớp 100%, BCP không lệch
```

**Kiểm Tra**:
```bash
# Kiểm tra Landing schema
SELECT COLUMN_NAME, ORDINAL_POSITION, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'ThuPhiDichVu'
ORDER BY ORDINAL_POSITION;

# So sánh với file BCP
# Số cột + thứ tự phải 100% khớp
```

(nguồn: PROJECT_CHRONICLE.md - ADR-23, ADR-31, ADR-32)

## 4. SQL Error 544 - IDENTITY_INSERT OFF

### Triệu Chứng
```
[14:40:10] [hanoi] [FactLoader] [ERROR] SQL Error 544
Msg 544, Level 16: Cannot insert explicit value for identity column
'[stg_nano_v2].[ThuPhi].[ThuPhiKey]' when IDENTITY_INSERT is set to OFF.
```

### Nguyên Nhân Gốc
```
Bảng Landing chứa cột IDENTITY:
├─ [ThuPhiKey] INT IDENTITY(1, 1) PRIMARY KEY
├─ [NgayDen] DATETIME
├─ [MaBN] VARCHAR(50)
└─ Khi INSERT row với ThuPhiKey value cụ thể → lỗi 544
```

(nguồn: PROJECT_CHRONICLE.md - ADR-37)

### Cách Khắc Phục

**Nguyên Tắc**: Bật `SET IDENTITY_INSERT` **trước** `executemany()`

```python
# Kiểm tra động
identity_check_sql = """
SELECT OBJECTPROPERTY(OBJECT_ID('stg_nano_v2.ThuPhiDichVu'), 'TableHasIdentity')
"""
result = ods_connection.execute(identity_check_sql).fetchval()
has_identity = (result == 1)

if has_identity:
    ods_connection.execute("SET IDENTITY_INSERT [stg_nano_v2].[ThuPhiDichVu] ON")

try:
    # Nạp dữ liệu
    ods_connection.cursor().executemany(insert_sql, rows)
    ods_connection.commit()
finally:
    if has_identity:
        ods_connection.execute("SET IDENTITY_INSERT [stg_nano_v2].[ThuPhiDichVu] OFF")
        ods_connection.commit()  # Commit để chốt trạng thái
```

**Quy Tắc**:
- **Luôn bao trong try/finally** để đảm bảo TẮT IDENTITY_INSERT
- Nếu không TẮT → session tiếp theo cũng bị ảnh hưởng
- Bật/tắt phải cùng connection

(nguồn: PROJECT_CHRONICLE.md - ADR-37)

## 5. Dữ Liệu Bẩn - Unicode & Ký Tự Ẩn

### Triệu Chứng
```
[14:45:00] [hanoi] [FactLoader] [ERROR] Data truncated on insert
[14:45:01] [hanoi] [FactLoader] [ERROR] Unexpected character at line 10,000
```

### Nguyên Nhân Gốc
```
Dữ Liệu HIS chứa:
├─ \n (newline)      trong cột text tự do (Notes)
├─ \r (carriage return)
├─ \t (tab)
└─ Binary character (0x00, 0xFF)

Khi BCP OUT text → File .txt:
├─ Delimiter = tab (\t)
├─ End of line = \n
├─ Dữ liệu chứa \n → xô lệch dòng, xô lệch cột
└─ BCP IN → Cast sai kiểu
```

(nguồn: PROJECT_CHRONICLE.md - ADR-17)

### Cách Khắc Phục

**Phương Pháp 1: BCP Unicode Flag**
```bash
# ❌ SAI - Dùng default text mode
bcp "SELECT * FROM Table" out data.txt -S SERVER -d DB -U user -P pass

# ✅ ĐÚNG - Dùng -w (wide char, UTF-16-LE)
bcp "SELECT * FROM Table" out data.txt -w -t\t -r\n -S SERVER -d DB -U user -P pass
# -w: Unicode wide (UTF-16-LE)
# -t\t: Tab delimiter
# -r\n: Newline terminator
```

**Phương Pháp 2: PyODBC (Khuyên Dùng)**
```python
# Không dùng BCP file, nạp trực tiếp ODBC
# PyODBC xử lý Unicode implicitly

cursor = prod_connection.cursor()
cursor.execute("SELECT * FROM [Table] WHERE NgayDen >= ? AND NgayDen <= ?", 
               effective_from_date, to_date)

staging_cursor = ods_connection.cursor()

while True:
    rows = cursor.fetchmany(1000)
    if not rows:
        break
    staging_cursor.executemany(insert_sql, rows)  # PyODBC handle Unicode
    ods_connection.commit()
```

**Lợi Ích ODBC**:
- ✅ Không file trung gian
- ✅ Unicode xử lý implicitly
- ✅ Không xô lệch cột
- ✅ Implicit casting tự động

(nguồn: PROJECT_CHRONICLE.md - ADR-17, ADR-34, ADR-35)

## 6. Lỗi Cơ Sở Dữ Liệu - Đứt Kết Nối

### Triệu Chứng
```
[14:50:00] [hanoi] [DimensionLoader] [ERROR] pyodbc.OperationalError
[08S01] Communication link failure
```

### Nguyên Nhân Gốc
- Connection timeout (default 30s)
- Network issue giữa app & DB server
- DB server restart giữa query

### Cách Khắc Phục

**Kiểm Tra Connection String**:
```python
# Thêm timeout & retry parameter vào .env
PROD_CONNECTION_HANOI=Driver={ODBC Driver 17 for SQL Server};Server=...;Database=...;Uid=...;Pwd=...;Connection Timeout=60;
```

**Implement Retry Logic**:
```python
import time

def execute_with_retry(connection, sql, max_retry=3):
    for attempt in range(max_retry):
        try:
            result = connection.execute(sql)
            return result
        except pyodbc.OperationalError as e:
            if attempt < max_retry - 1:
                log.warning(f"Retry {attempt+1}/{max_retry} sau 5 giây...")
                time.sleep(5)
            else:
                raise
```

**Kiểm Tra Kết Nối**:
```bash
# Từ terminal, test kết nối
sqlcmd -S SERVER_NAME -d DATABASE -U user -P password
> SELECT 1;
> GO

# Nếu OK → connection string đúng
```

(nguồn: PROJECT_CHRONICLE.md)

## 7. Hard Delete Sai Phạm Vi - Xóa Nhầm Dữ Liệu

### Triệu Chứng
```
[Trước chạy] ODS có 1000 rows
[Sau chạy]  ODS có 500 rows (400 rows bị xóa sai!)
[Log]       MERGE output: "200 DELETED, 300 INSERTED, 0 UPDATED"
```

### Nguyên Nhân Gốc
```sql
-- ❌ SAI - Xóa toàn bộ dữ liệu không có matching source
MERGE INTO [ODS].[ThuPhi] AS Target
USING [Landing].[ThuPhi] AS Source
ON Target.NgayDen = Source.NgayDen
WHEN NOT MATCHED BY SOURCE THEN DELETE;  -- Xóa toàn bộ cũ!
```

### Cách Khắc Phục

**Thêm Điều Kiện Hard Delete D-3**:
```sql
-- ✅ ĐÚNG - Chỉ xóa D-3
MERGE INTO [ODS].[ThuPhi] AS Target
USING [Landing].[ThuPhi] AS Source
ON Target.NgayDen = Source.NgayDen
  AND Target.NguonDuLieuKey = Source.NguonDuLieuKey  -- Cô lập cơ sở
WHEN NOT MATCHED BY SOURCE
  AND CAST(Target.NgayDen AS DATE) >= DATEADD(DAY, -3, CAST(GETDATE() AS DATE))
THEN
    DELETE;  -- Chỉ xóa trong D-3
```

**Kiểm Tra Log**:
```
[14:55:00] [MERGE] DELETED: 50 rows (trong D-3)
[14:55:00] [MERGE] INSERTED: 150 rows
[14:55:00] [MERGE] UPDATED: 20 rows
→ OK, số lượng hợp lý
```

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 8. Query Chậm - Timeout

### Triệu Chứng
```
[15:00:00] [START] Nạp ThuPhiDichVu (1 tuần)...
[15:15:00] ... (15 phút vẫn chưa xong)
[15:30:00] Timeout 30 min - Dừng
```

### Nguyên Nhân Gốc
- Query SELECT từ Production quá lâu
- Không có index trên cột ngày
- DB server bị quá tải

### Cách Khắc Phục

**Kiểm Tra Query Tốc Độ**:
```sql
-- Test trực tiếp trên Production
SET STATISTICS TIME ON;

SELECT * FROM [ThuPhiDichVu]
WHERE NgayDenKham >= '2026-05-15' AND NgayDenKham <= '2026-05-22';

SET STATISTICS TIME OFF;

-- Xem "SQL Server Execution Times"
```

**Tối Ưu Query**:
```sql
-- Tạo index trên cột ngày nếu chưa có
CREATE INDEX idx_ngay_denkham 
ON [ThuPhiDichVu] ([NgayDenKham], [MaBenhNhan]);

-- Hoặc hạ batch size để output nhỏ hơn
SELECT TOP 10000 * FROM [ThuPhiDichVu] WHERE NgayDenKham >= ...
```

**Increase Timeout**:
```python
# Trong code
staging_cursor.timeout = 300  # 5 phút
ods_connection.timeout = 300
```

(nguồn: PROJECT_CHRONICLE.md)

---

## Tóm Tắt Checklist Debug

| Vấn Đề | Checklist |
|---|---|
| **Treo** | ✓ TRUNCATE có commit()? ✓ BCP timeout? ✓ Lock process? |
| **MemoryError** | ✓ fast_executemany? ✓ batch_size? ✓ Cột MAX? |
| **Cast Error 22005** | ✓ Số cột khớp? ✓ Thứ tự cột? ✓ Whitelist? |
| **Identity 544** | ✓ Bảng có IDENTITY? ✓ SET ON/OFF? ✓ Đúng bảng? |
| **Dữ Liệu Bẩn** | ✓ Unicode flag? ✓ Ký tự ẩn? ✓ ODBC vs BCP? |
| **Xóa Sai** | ✓ Hard delete D-3? ✓ NguonDuLieuKey? ✓ Where clause? |

---

## Các Trang Liên Quan

- [[quyet-dinh-kien-truc-40adr]] - Chi tiết từng ADR giải pháp
- [[fact-loader]] - Luồng Fact incremental
- [[dimension-loader]] - Luồng Dimension full-load
- [[chinh-sach-bao-mat-db]] - Guard rail bảo mật
