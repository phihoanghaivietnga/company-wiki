# Chính Sách Bảo Mật DB - Điều Luật Kết Nối

**Tóm tắt**: Hệ thống ETL áp dụng nguyên tắc bảo mật nghiêm ngặt: Production được bảo vệ với quyền CHỈ ĐỌC, còn Staging/Datamart được phép thay đổi dữ liệu. Tất cả log phải che giấu mật khẩu, connection string nhạy cảm.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (agents.md section 3.1)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Nguyên Tắc Bảo Mật Chung

Hệ thống hoạt động theo mô hình **Three-Zone Security**:

```
┌────────────────────────────────────────────────────────────────┐
│ Production HIS (Zone: PROTECTED)                              │
├────────────────────────────────────────────────────────────────┤
│ ✅ ALLOW: SELECT, BCP OUT                                     │
│ ❌ DENY:  INSERT, UPDATE, DELETE, TRUNCATE, ALTER, CREATE     │
└────────────────────────────────────────────────────────────────┘
                              │
                              ↓
┌────────────────────────────────────────────────────────────────┐
│ Staging / ODS (Zone: CONTROLLED)                              │
├────────────────────────────────────────────────────────────────┤
│ ✅ ALLOW: TRUNCATE, INSERT, UPDATE, DELETE, MERGE, ALTER      │
│ ❌ DENY:  (nothing - full write access)                        │
└────────────────────────────────────────────────────────────────┘
                              │
                              ↓
┌────────────────────────────────────────────────────────────────┐
│ Datamart (Zone: CONTROLLED)                                    │
├────────────────────────────────────────────────────────────────┤
│ ✅ ALLOW: MERGE, DELETE (with safeguards), INSERT, UPDATE      │
│ ❌ DENY:  (nothing - full write access)                        │
└────────────────────────────────────────────────────────────────┘
```

(nguồn: agents.md - 3.1)

## 2. Zone 1: Production (PROTECTED)

### Quyền Cho Phép (Whitelist)

✅ **CHỈ CHO PHÉP**:
```sql
SELECT ...
BCP OUT (từ command line)
```

**Mục Đích**:
- Trích xuất dữ liệu (không sửa)
- Đảm bảo dữ liệu Production không bị thay đổi bởi ETL

(nguồn: agents.md - 3.1)

### Lệnh Cấm (Blacklist)

❌ **TUYỆT ĐỐI CẤM**:
```sql
INSERT INTO ...              -- Thêm dữ liệu
UPDATE ...                   -- Sửa dữ liệu
DELETE ...                   -- Xóa dữ liệu
TRUNCATE ...                 -- Xóa toàn bộ bảng
ALTER TABLE ...              -- Sửa cấu trúc
CREATE TABLE ...             -- Tạo bảng
DROP TABLE ...               -- Xóa bảng
```

**Lý Do**:
- Production là **nguồn dữ liệu gốc** của bệnh viện
- Một khi sửa sai → mất dữ liệu tác nghiệp thực
- **Không có cách khôi phục** (ngoài backup)

(nguồn: agents.md - 3.1)

### Guard Rail: Kiểm Tra Production Query

```python
# Pseudocode trong BaseLoader / DimensionLoader
def execute_sql_production(connection, sql):
    # Kiểm tra trước khi thực thi
    normalized_sql = sql.strip().upper()
    
    if not normalized_sql.startswith("SELECT"):
        raise RuntimeError(
            f"Production query phải bắt đầu với SELECT. "
            f"Nhận được: {normalized_sql[:50]}"
        )
    
    # Chạy query an toàn
    cursor = connection.cursor()
    cursor.execute(sql)
    return cursor
```

**Ý Nghĩa**:
- Runtime guard để phát hiện query sai
- Log chi tiết khi phát hiện lỗi
- Dừng luồng ETL để tránh lan lỗi

(nguồn: PROJECT_CHRONICLE.md - ADR-11)

## 3. Zone 2 & 3: Staging / ODS / Datamart (CONTROLLED)

### Quyền Cho Phép (Whitelist)

✅ **CHO PHÉP ĐỀY ĐỦ**:
```sql
TRUNCATE TABLE ...           -- Xóa toàn bộ (chuẩn bị nạp)
INSERT INTO ...              -- Thêm dữ liệu từ Prod
UPDATE ...                   -- Cập nhật liên kết / FK
DELETE ...                   -- Xóa dữ liệu cũ (D-3 safeguard)
MERGE ...                    -- UPSERT dữ liệu
ALTER TABLE ...              -- Sửa cấu trúc (nếu cần)
CREATE TABLE ...             -- Tạo bảng tạm
```

**Mục Đích**:
- ETL cần toàn quyền để xử lý dữ liệu
- Không có hạn chế để tránh gãy luồng

(nguồn: agents.md - 3.1)

### Safeguard: Hard Delete

```sql
-- ✅ CHO PHÉP: Hard delete D-3
DELETE FROM [Datamart].[FactThuPhi]
WHERE NgayDenKham >= DATEADD(DAY, -3, CAST(GETDATE() AS DATE))
  AND NguonDuLieuKey = 10;

-- ❌ CẤM: Hard delete toàn bộ lịch sử
DELETE FROM [Datamart].[FactThuPhi];

-- ❌ CẤM: Hard delete nhầm cơ sở
DELETE FROM [Datamart].[FactThuPhi]
WHERE NgayDenKham < '2020-01-01';  -- Quá lâu
```

**Quy Tắc**:
1. **Thời gian**: Chỉ xóa D-3 (3 ngày gần nhất), không xóa cũ hơn
2. **Cơ sở**: Bắt buộc kiểm tra `NguonDuLieuKey` hoặc `MaCoSo`
3. **Template**: Safeguard phải được hardcode vào SQL template

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 4. Log Sanitization - Che Giấu Thông Tin Nhạy Cảm

### 4.1 Cấm Log Thông Tin Nhạy Cảm

❌ **CẤM log**:
```python
# SAI: Log connection string
log.info(f"Connecting to {connection_string}")
# Output: Connecting to Driver=...;PWD=MyPassword123

# SAI: Log full query
log.debug(f"Executing: {sql}")
# Output: Executing: SELECT * FROM ... WHERE PWD='abc123'

# SAI: Log BCP command
log.info(f"Running BCP: {' '.join(command)}")
# Output: Running BCP: bcp MyDB..MyTable in data.txt -P MyPassword
```

**Vì Sao**:
- Password, secrets, usernames là thông tin nhạy cảm
- Nếu log được lưu vào file / stdout → dễ bị lộ
- Có thể bị đọc bởi không phải người dùng authorized

(nguồn: PROJECT_CHRONICLE.md - ADR-15, ADR-24)

### 4.2 Phương Pháp Log Sanitize

✅ **ĐÚNG: Log thông điệp chung chung**:
```python
log.info("Connecting to Production database...")
log.info("Executing SELECT query...")
log.info("Bắt đầu thực thi BCP OUT từ Production...")

# Nếu lỗi:
log.error("BCP failed: Tiến trình BCP thất bại. "
          "Đã ẩn chi tiết command để bảo mật.")
```

**Lợi Ích**:
- Log vẫn giúp debug luồng (timing, sequence)
- Không lộ credential
- Cho phép mover log file mà không lo security

(nguồn: PROJECT_CHRONICLE.md - ADR-15, ADR-24)

### 4.3 Hàm Mask Password

```python
def mask_connection_string(conn_str):
    """Che giấu PWD= trong connection string"""
    import re
    return re.sub(r'PWD=.*?;', 'PWD=***;', conn_str, flags=re.IGNORECASE)

def mask_bcp_command(command_list):
    """Che giấu -P password trong BCP command"""
    masked = []
    for i, part in enumerate(command_list):
        if part.lower() == '-p' and i+1 < len(command_list):
            masked.append('-P')
            masked.append('***')
            continue
        masked.append(part)
    return masked
```

**Dùng Cho**:
- Log lỗi exception khi cần in command (nhưng che mật khẩu)
- Giúp dev debug mà không tiết lộ credential

(nguồn: PROJECT_CHRONICLE.md - ADR-16)

## 5. Connection Management - Quản Lý Kết Nối

### 5.1 Context Manager Pattern

```python
# ✅ ĐÚNG
from contextlib import contextmanager

@contextmanager
def get_db_connection(connection_string):
    connection = pyodbc.connect(connection_string)
    try:
        yield connection
    finally:
        connection.close()  # Đảm bảo đóng

# Dùng:
with get_db_connection(PROD_CONNECTION) as prod_conn:
    cursor = prod_conn.cursor()
    cursor.execute("SELECT ...")
    # Connection tự động đóng khi thoát block
```

**Lợi Ích**:
- ✅ Connection luôn được đóng (không leak)
- ✅ Tự động rollback nếu lỗi (không commit dữ liệu bẩn)
- ✅ An toàn trong multi-thread

(nguồn: PROJECT_CHRONICLE.md - ADR-02)

### 5.2 Transaction Boundary & Commit

```python
# ✅ ĐÚNG: Commit sau mỗi hoạt động quan trọng
connection.execute("TRUNCATE TABLE [landing].[Fact]")
connection.commit()  # Giải phóng lock ngay

# Nạp dữ liệu theo batch
for batch in chunks:
    cursor.executemany(insert_sql, batch)
    connection.commit()  # Commit mỗi batch

# ❌ SAI: Giữ transaction quá lâu
connection.execute("TRUNCATE TABLE [landing].[Fact]")
# ... làm việc khác 30 giây ...
connection.execute("INSERT ...")
# Mới commit -> lock giữ 30 giây, other sessions chờ
connection.commit()
```

**Quy Tắc**:
- Commit sau TRUNCATE để giải phóng lock ngay
- Commit mỗi batch để tránh treo session
- Không giữ transaction lâu hơn cần thiết

(nguồn: PROJECT_CHRONICLE.md - ADR-14, ADR-22)

## 6. Deadlock Prevention

### Vấn Đề: TRUNCATE + BCP Hang

```
Session A (pyodbc)         Session B (BCP process)
├─ EXECUTE TRUNCATE         └─ Chờ lock từ Session A
├─ Giữ Sch-M lock
└─ (không commit)
    └─ BCP chờ để INSERT
       (vĩnh viễn treo)
```

### Giải Pháp

```python
# ✅ ĐÚNG
connection.execute("TRUNCATE TABLE [stg].[Fact]")
connection.commit()  # Giải phóng lock ngay
time.sleep(0.1)     # Chờ session xử lý

# Giờ BCP có thể INSERT vào bảng
subprocess.run(bcp_command, ...)
```

**Chốt Chặn**:
1. TRUNCATE → commit() ngay
2. Đợi 0.1s để DB xử lý lock release
3. BCP INSERT thành công

(nguồn: PROJECT_CHRONICLE.md - ADR-14, ADR-22)

## 7. Semaphore - Bảo Vệ Production

### Vấn Đề: Quá Tải DB

```
UI Manual Runner: Chạy 10 query đồng thời
├─ SELECT Dim A, B, C, D, E...
├─ SELECT Fact X, Y, Z...
└─ Cộng lại 20 concurrent queries
    → Production DB overwhelmed
    → Report user bị hang
```

### Giải Pháp: Semaphore

```python
import asyncio

MAX_CONCURRENT_CONNECTIONS = 5  # Tối đa 5 query cùng lúc
semaphore = asyncio.Semaphore(MAX_CONCURRENT_CONNECTIONS)

async def execute_with_limit(query, facility):
    async with semaphore:
        # Chỉ 5 query chạy cùng lúc
        result = await db.execute(query)
        return result

# Dùng:
results = await asyncio.gather(
    execute_with_limit(q1, 'hanoi'),
    execute_with_limit(q2, 'hcm'),
    ... (tối đa 5 concurrent)
)
```

**Lợi Ích**:
- ✅ Production DB không bị quá tải
- ✅ Report user vẫn được phục vụ
- ✅ ETL và UI cân bằng tài nguyên

(nguồn: PROJECT_CHRONICLE.md - ADR-03)

## 8. Audit Trail & Compliance

### Log Bắt Buộc

| Sự Kiện | Log | Ví Dụ |
|---|---|---|
| Bắt đầu Loader | [TIMESTAMP] [FACILITY] [LOADER] [START] | 2026-05-22 14:30:45.123 [hanoi] [DimensionLoader] [START] |
| Kết thúc thành công | [TIMESTAMP] [FACILITY] [LOADER] [SUCCESS] | 2026-05-22 14:30:50.456 [hanoi] [DimensionLoader] [SUCCESS] |
| Lỗi | [TIMESTAMP] [FACILITY] [LOADER] [ERROR] [MESSAGE] | 2026-05-22 14:30:52.789 [hanoi] [DimensionLoader] [ERROR] TRUNCATE failed |

**Mục Đích**:
- Trace who did what when
- Tìm nguyên nhân khi xảy ra sự cố
- Tuân thủ audit requirements

(nguồn: GEM_AUTO_PIPELINE.md)

---

## Các Trang Liên Quan

- [[kien-truc-da-tang]] - Bảo vệ từng tầng
- [[orchestrator-dong-bo]] - Thực thi chính sách
- [[dimension-loader]] - Guard on Production
- [[fact-loader]] - Hard delete safeguard
