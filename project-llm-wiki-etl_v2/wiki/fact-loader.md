# Fact Loader - Incremental 3-Hop

**Tóm tắt**: Module `FactLoader` nạp dữ liệu phát sinh (transactional) theo chế độ incremental với cửa sổ trượt D-3 (3 ngày gần nhất), qua 3 chặng: Global Staging → ODS facility → Datamart. Áp dụng hard delete an toàn, cô lập cơ sở, và bảo vệ tài chính bằng công thức fallback.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_CODE_MAP.md, PROJECT_CHRONICLE.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Phạm Vi Bảng Fact Incremental

**Danh Sách Fact được Hỗ Trợ**:

| Bảng Fact | Cột Ngày | Lookback (D) |
|---|---|---|
| ThuPhiDichVu | `NgayDenKham` | D-3 |
| ThuPhiBaoHiem | `NgayDenKham` | D-3 |
| ThuPhiTangGiam | `NgayDenKham` | D-3 |
| ThuPhiGoi | `NgayThu` | D-3 |
| DoThiLuc | `NgayDo` | D-3 |
| HoSoKhamBenhNgoaiTru | `NgayVaoKham` | D-3 |

(nguồn: GEM_CODE_MAP.md - ADR-21)

**Lưu ý**: Danh sách được cấu hình trong `config/tables.yaml` → `incremental_tables`. Có thể mở rộng bằng cách thêm entry YAML mà không cần sửa code.

(nguồn: PROJECT_CHRONICLE.md - ADR-21)

## 2. Cửa Sổ Trượt D-3 (3 Ngày Gần Nhất)

### Công Thức
```
effective_from_date = from_date - lookback_days
where_clause: "WHERE NgayDenKham >= @effective_from_date AND NgayDenKham <= @to_date"
```

**Ví Dụ**:
- Hôm nay: 2026-05-22
- `from_date = 2026-05-20` (hôm kia)
- `lookback_days = 3` (từ config)
- `effective_from_date = 2026-05-17` (cách đây 3 ngày)
- Query: `WHERE NgayDenKham >= '2026-05-17' AND NgayDenKham <= '2026-05-20'`

### Mục Đích

- **Overlap 3 ngày**: Bắt được dữ liệu muộn hoặc sửa chữa từ 3 ngày trước
- **Hard Delete An Toàn**: Chỉ xóa dữ liệu cũ hơn 3 ngày nếu cần
- **Tránh Xóa Sai**: Không xóa lịch sử quá khứ xa

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 3. Kiến Trúc 3-Hop

```
Production (D-3 to today)
    ↓ [Tầng 1: ODBC bulk select + nạp]
Global Staging (stg_nano_v2) - TRANSIENT
    [TRUNCATE đầu + dữ liệu thô]
    ↓ [Tầng 2: MERGE + hard delete D-3]
ODS Facility (hanoi_hisnano_v2, etc.)
    [Lịch sử bền vững per facility]
    ↓ [Tầng 3: MERGE + hard delete D-3]
Datamart
    [Dữ liệu cuối cùng cho báo cáo]
```

(nguồn: GEM_DATA_FLOW.md)

## 4. Luồng Tầng 1: Prod → Global Staging

### 4.1 Whitelist Cột (selected_columns)

```yaml
# config/tables.yaml
incremental_tables:
  ThuPhiDichVu:
    selected_columns:
      - NgayDenKham
      - MaBenhNhan
      - NgayThuPhiBaoHiem
      - TongTien
      - TongGiam
      - TongTang
```

**Ý Nghĩa**:
- Chỉ bốc cột cần thiết (không lấy toàn bộ)
- Giảm tải bộ nhớ từ giai đoạn đầu
- Tránh cột bẩn hoặc TEXT/NVARCHAR(MAX) không cần

(nguồn: PROJECT_CHRONICLE.md - ADR-31)

### 4.2 Xây Dựng Dynamic SELECT

```sql
SELECT
  [NgayDenKham],
  [MaBenhNhan],
  [NgayThuPhiBaoHiem],
  [TongTien],
  [TongGiam],
  [TongTang],
  1 AS [CoSoKey],              -- Tenant injection
  10 AS [NguonDuLieuKey],
  'HN' AS [MaCoSo]
FROM [Production].[dbo].[ThuPhiDichVu]
WHERE NgayDenKham >= @from_date AND NgayDenKham <= @to_date
```

**Quy Tắc**:
- Cột từ Whitelist được khai báo rõ ràng
- **Tenant Injection**: 3 cột định danh thêm vào cuối
- **Cửa sổ ngày**: Bắt buộc có điều kiện `WHERE` theo cột ngày

(nguồn: PROJECT_CHRONICLE.md - ADR-31, ADR-32)

### 4.3 TRUNCATE Landing Đầu Luồng

```python
# Bước 1: TRUNCATE Landing trước nạp
ods_connection.execute("TRUNCATE TABLE [stg_nano_v2].[ThuPhiDichVu]")
ods_connection.commit()  # Giải phóng lock ngay
```

**Mục Đích**:
- Xóa dữ liệu cũ từ lần chạy trước
- Tránh nhiễm độc chéo giữa các facility

(nguồn: GEM_DATA_FLOW.md)

### 4.4 ODBC Bulk Insert (PyODBC chunking)

```python
# Pseudocode
prod_connection.fast_executemany = False  # Tắt vĩnh viễn

select_sql = "SELECT ... WHERE NgayDenKham >= ... AND NgayDenKham <= ..."
cursor = prod_connection.cursor()
cursor.execute(select_sql)

staging_cursor = ods_connection.cursor()

while True:
    rows = cursor.fetchmany(1000)  # batch_size từ YAML
    if not rows:
        break
    
    insert_sql = "INSERT INTO [stg_nano_v2].[ThuPhiDichVu] ([Col1], ...) VALUES (?, ?, ...)"
    staging_cursor.executemany(insert_sql, rows)
    
    ods_connection.commit()  # Commit mỗi batch
```

**Điểm Quan Trọng**:
- **fast_executemany = False**: Bảo vệ bộ nhớ tuyệt đối (bó chặt ngay cả khi chậm)
- **Chunking 1000 rows**: Giảm tải RAM per batch
- **commit() sau mỗi batch**: Giải phóng transaction, tránh lock

(nguồn: PROJECT_CHRONICLE.md - ADR-38)

### 4.5 TRUNCATE Landing Cuối Luồng

```python
# finally block
try:
    # ... nạp dữ liệu ...
finally:
    ods_connection.execute("TRUNCATE TABLE [stg_nano_v2].[ThuPhiDichVu]")
    ods_connection.commit()
```

**Mục Đích**:
- Dọn sạch Landing ngay cả khi có lỗi
- Tránh rò rỉ dữ liệu sang facility kế tiếp

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 5. Luồng Tầng 2: Global Staging → ODS Facility

### 5.1 MERGE với Hard Delete An Toàn

```sql
MERGE INTO [hanoi_hisnano_v2].[ThuPhiDichVu] AS Target
USING [stg_nano_v2].[ThuPhiDichVu] AS Source
ON Target.NgayDenKham = Source.NgayDenKham
  AND Target.MaBenhNhan = Source.MaBenhNhan
WHEN MATCHED THEN
    UPDATE SET Target.TongTien = Source.TongTien, ...
WHEN NOT MATCHED BY SOURCE
  AND CAST(Target.NgayDenKham AS DATE) >= DATEADD(DAY, -3, CAST(GETDATE() AS DATE))
THEN
    DELETE
WHEN NOT MATCHED THEN
    INSERT (...) VALUES (...);
```

**Quy Tắc Hard Delete**:
- ❌ **CẤM xóa toàn bộ** dữ liệu cũ
- ✅ **CHỈ xóa** dữ liệu trong cửa sổ D-3
- ✅ **Giữ lịch sử** ngoài 3 ngày để audit trail

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

### 5.2 Cô Lập Cơ Sở

```sql
-- Condition ON phải chứa NguonDuLieuKey để tránh match nhầm cơ sở khác
ON Target.NgayDenKham = Source.NgayDenKham
  AND Target.MaBenhNhan = Source.MaBenhNhan
  AND Target.NguonDuLieuKey = Source.NguonDuLieuKey  -- CÓ ĐIỀU KIỆN NÀY
```

**Mục Đích**:
- Tránh update/delete nhầm lên dữ liệu facility khác
- Multi-tenant isolation

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 6. Luồng Tầng 3: ODS → Datamart

### 6.1 MERGE SQL Template

```sql
-- Template không được sửa nội dung, chỉ render placeholder
-- File: src/db/templates/sql/merge_fact_thuphichvu_3in1.sql
MERGE INTO [Datamart].[FactThuPhiDichVu] AS Target
USING [hanoi_hisnano_v2].[ThuPhiDichVu] AS Source
ON Target.NgayDenKham = Source.NgayDenKham
  AND Target.MaBenhNhan = Source.MaBenhNhan
  AND Target.NguonDuLieuKey = Source.NguonDuLieuKey
WHEN MATCHED THEN
    UPDATE SET Target.TongTien = Source.TongTien, ...
WHEN NOT MATCHED BY SOURCE
  AND CAST(Target.NgayDenKham AS DATE) >= DATEADD(DAY, -3, CAST(GETDATE() AS DATE))
THEN
    DELETE
WHEN NOT MATCHED THEN
    INSERT (...) VALUES (...);
```

**Quy Tắc**:
- Template được quản lý **riêng biệt** (không generate động)
- Chỉ render placeholder `{facility}`, `{schema}` nếu cần
- **Không được sửa** nội dung SQL template
- Tất cả thay đổi logic phải **qua code review** Gem

(nguồn: PROJECT_CHRONICLE.md - ADR-21)

### 6.2 Batching TOP (10,000)

```sql
-- Nếu bảng lớn, có thể split thành batch
DELETE TOP (10000) FROM [Datamart].[FactThuPhiDichVu]
WHERE NgayDenKham < DATEADD(DAY, -3, CAST(GETDATE() AS DATE))
  AND NguonDuLieuKey = @nguon_dulieu_key;
```

**Mục Đích**:
- Tránh lock bảng quá lâu khi delete lớn
- Giảm overhead transaction log

(nguồn: PROJECT_CHRONICLE.md - ADR-08)

## 7. Early Arriving Facts - Fallback Seed Key

### Vấn Đề
- Fact đến trước dimension → Foreign key constraint fail
- Ví dụ: `ThuPhiDichVu` đến nhưng `DimDichVu` chưa có

### Giải Pháp
```sql
-- Khi insert fact, nếu DichVuKey không tìm thấy trong Dim:
MERGE INTO [Datamart].[FactThuPhiDichVu]
USING (
    SELECT 
        NgayDenKham,
        ISNULL(DimDichVu.DichVuKey, -1) AS DichVuKey,  -- Fallback -1
        ...
    FROM [ODS].[ThuPhiDichVu]
    LEFT JOIN [Datamart].[DimDichVu] ON ...
) AS Source
```

**Fallback Seed Keys**:
- `LuotKhamKey = -1` (chưa có LuotKham)
- `BenhNhanKey = -1` (chưa có BenhNhan)
- `DichVuKey = -1` (chưa có DichVu)

**Lợi Ích**:
- Fact không bị rớt bản ghi
- Đợi dimension sau được nạp, rồi update FK

(nguồn: PROJECT_CHRONICLE.md - ADR-09)

## 8. Công Thức Tài Chính 3-in-1

### ThuPhiDichVu
```
TongTienSauTangGiam = TongTien - ISNULL(TongGiam, 0) + ISNULL(TongTang, 0)
```

### ThuPhiBaoHiem
```
TongTienSauTangGiam = TongTien + ISNULL(TienChenhLech, 0)
```

### ThuPhiTangGiam
```
(Là bảng phụ ghi chi tiết các khoản tăng giảm)
```

**Bảo Vệ**:
- Tất cả công thức đều **được hardcode trong SQL template**
- Code Python **không được sửa** công thức
- Nếu cần thay đổi công thức → Gem phê duyệt, sửa template

(nguồn: PROJECT_CHRONICLE.md - ADR-09)

## 9. Cụm 3 Bảng Doanh Thu (Manual Runner)

### Nhóm ThuPhiDichVu

Khi người dùng nhấn "Run ThuPhiDichVu" trên UI:
```python
# FactLoader tự động xử lý 3 bảng tuần tự
CLUSTER = {
    "ThuPhiBaoHiem",
    "ThuPhiTangGiam", 
    "ThuPhiDichVu"
}

for table_name in CLUSTER:
    # Nạp từng bảng qua 3 chặng
    load_fact_table(table_name)
```

**Mục Đích**:
- Người dùng chỉ cần chọn "ThuPhiDichVu"
- Hệ thống tự động nạp đủ 3 bảng doanh thu
- Tránh nhầm lẫn, đảm bảo tính nhất quán tài chính

(nguồn: PROJECT_CHRONICLE.md - ADR-21, 2026-05-21)

---

## Các Trang Liên Quan

- [[orchestrator-dong-bo]] - Điều phối Fact
- [[dimension-loader]] - Nạp Dimension full-load
- [[kien-truc-da-tang]] - Kiến trúc 3 chặng chung
- [[loi-va-khac-phuc]] - Xử lý lỗi incremental
- [[huong-dan-manual-runner]] - UI chạy Fact manual
