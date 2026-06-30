# Dimension Loader - Full-Load 2-Hop

**Tóm tắt**: Module `DimensionLoader` nạp danh mục đầy đủ (full-load) từ Production sang Datamart qua 2 chặng: ODS facility (Tầng 3) → Datamart (Tầng 4). Chỉ hỗ trợ 4 dimension chính và áp dụng tenant injection để giữ định danh cơ sở.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_CODE_MAP.md, PROJECT_CHRONICLE.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Phạm Vi Dimension

**4 Dimension Chính** được hỗ trợ:

| Dimension | Bảng Prod | SQL Template |
|---|---|---|
| **DimBenhNhan** | `DMBenhNhan` | `DimBenhNhan_merge.sql` |
| **DimBenh** | `DMBenh` | `DimBenh_merge.sql` |
| **DimLoaiGoiDichVu** | `LoaiGoiDichVuNT` | `DimLoaiGoiDichVu_merge.sql` |
| **DimDichVu** | 3 bảng: `DMLoaiDichVu`, `DMDichVu`, `DMDichVuChiTiet` | `dim_dich_vu_merge.sql` |

(nguồn: PROJECT_CHRONICLE.md - ADR-10)

**Lưu ý**: 
- `DimLuotKham` **đã bị gỡ bỏ** để tránh `TRUNCATE` nhầm lên bảng có bản chất incremental
- Các dimension khác không được hỗ trợ hiện tại để giữ tính chắc chắn

(nguồn: PROJECT_CHRONICLE.md - ADR-10)

## 2. Kiến Trúc 2-Hop

```
Production
    ↓ (full select)
Global Staging (stg_nano_v2)
    [Không dùng, bỏ qua]
    ↓
ODS Facility (hanoi_hisnano_v2, etc.)
    [TRUNCATE + ODBC executemany]
    ↓
Datamart
    [MERGE SQL template]
```

**Thực tế chính**:
- **Tầng 2 (Global Staging)**: Không dùng cho Dimension
- **Tầng 3 (ODS)**: Nơi `TRUNCATE` và nạp thô từ Production
- **Tầng 4 (Datamart)**: Nơi MERGE kết quả cuối cùng

(nguồn: GEM_CODE_MAP.md)

## 3. Luồng Chi Tiết

### Bước 1: TRUNCATE ODS
```sql
TRUNCATE TABLE [hanoi_hisnano_v2].[DimBenhNhan]
```

**Quy tắc**:
- `TRUNCATE` xóa toàn bộ dữ liệu hiện tại
- Ngay sau `TRUNCATE` phải có `connection.commit()` để giải phóng lock
- Tránh treo session khi pyodbc giữ lock và tiến trình khác chờ

(nguồn: PROJECT_CHRONICLE.md - ADR-14)

### Bước 2: ODBC Bulk Copy (SELECT -> ODS)
```python
# Pseudocode
cursor = production_connection.cursor()
select_sql = "SELECT [Col1], [Col2], ... FROM [DMBenhNhan] WHERE ..."
cursor.execute(select_sql)

# Nạp dữ liệu chunking để an toàn bộ nhớ
while True:
    rows = cursor.fetchmany(1000)  # batch_size từ YAML
    if not rows:
        break
    
    # Tiêm 3 cột tenant vào cuối dữ liệu
    enriched_rows = []
    for row in rows:
        enriched_row = tuple(row) + (co_so_key, nguon_du_lieu_key, ma_co_so)
        enriched_rows.append(enriched_row)
    
    # Insert vào ODS
    insert_sql = "INSERT INTO [ODS].[DimBenhNhan] (...) VALUES (...)"
    ods_connection.cursor().executemany(insert_sql, enriched_rows)

ods_connection.commit()
```

**Điểm Quan Trọng**:
- **Chunk Size**: Đọc từ `config/tables.yaml` `etl_settings.odbc_chunk_size`
- **Tenant Injection**: Tự động thêm 3 cột `NguonDuLieuKey`, `CoSoKey`, `MaCoSo`
- **fast_executemany = False**: Tắt vĩnh viễn để bảo vệ bộ nhớ với cột MAX
- **commit() theo lô**: Giải phóng transaction sau mỗi batch

(nguồn: PROJECT_CHRONICLE.md - ADR-38)

### Bước 3: MERGE ODS -> Datamart
```sql
-- Dùng SQL template có sẵn (ví dụ: DimBenhNhan_merge.sql)
-- Không chỉnh sửa file template
MERGE INTO [Datamart].[DimBenhNhan] AS Target
USING [hanoi_hisnano_v2].[DimBenhNhan] AS Source
ON Target.BenhNhanKey = Source.BenhNhanKey
WHEN MATCHED THEN
    UPDATE SET Target.TenBenhNhan = Source.TenBenhNhan, ...
WHEN NOT MATCHED THEN
    INSERT (...) VALUES (...);
```

**Quy Tắc**:
- Chỉ render placeholder (`{facility}`, `{schema}`, ...)
- Không sửa nội dung SQL template
- Log cặp `[START]` / `[SUCCESS]` để giám sát

(nguồn: PROJECT_CHRONICLE.md - ADR-21)

## 4. Quy Lệ Đặc Biệt: DimDichVu 3 Bảng

**Tính Chất Đặc Biệt**:
- DimDichVu tương ứng với 3 bảng Production: `DMLoaiDichVu`, `DMDichVu`, `DMDichVuChiTiet`
- 3 bảng này có quan hệ cha-con (hierarchical)

**Luồng Bắt Buộc**:
```
Bước 1: Nạp 3 bảng vào ODS tuần tự
├── DMLoaiDichVu -> [ODS].[DMLoaiDichVu]
├── DMDichVu -> [ODS].[DMDichVu]
└── DMDichVuChiTiet -> [ODS].[DMDichVuChiTiet]

Bước 2: Chỉ sau khi 3 bảng hoàn tất
└── MERGE 3 bảng -> [Datamart].[DimDichVu] (dùng template dim_dich_vu_merge.sql)
```

**Điểm Quan Trọng**:
- Bắt buộc **tuần tự** 3 bảng, không song song
- Chỉ sau khi ODS có đủ 3 bảng mới chạy MERGE Datamart
- Tránh rủi ro dữ liệu không đầy đủ khi MERGE

(nguồn: PROJECT_CHRONICLE.md - ADR-10)

## 5. Tenant Injection - Tiêm Định Danh Cơ Sở

### Công Thức
```python
# Từ config/tables.yaml
facilities:
  hanoi:
    co_so_key: 1
    nguon_du_lieu_key: 10
    ma_co_so: "HN"

# Khi nạp, mỗi row từ Production được tiêm thêm:
enriched_row = (col1, col2, ..., coln, 1, 10, "HN")
#                                        ↑  ↑   ↑
#                                  co_so_key | nguon_du_lieu_key | ma_co_so
```

### Mục Đích
- Bảo đảm tất cả dữ liệu ODS có định danh cơ sở
- Tránh NULL khi MERGE Datamart
- Hỗ trợ multi-tenant query: `SELECT * FROM [Datamart] WHERE MaCoSo = 'HN'`

### Cột Tiêm
| Cột | Kiểu | Ý Nghĩa |
|---|---|---|
| `CoSoKey` | INT | Khóa facility trong ODS |
| `NguonDuLieuKey` | INT | Khóa nguồn dữ liệu |
| `MaCoSo` | VARCHAR(50) | Mã facility text (HN, HCM, ...) |

(nguồn: PROJECT_CHRONICLE.md - ADR-20)

## 6. Bảo Vệ Production

**Nguyên Tắc**:
- Connection Production **chỉ dùng** cho `SELECT`
- Mọi lệnh write/update/delete **cấm hoàn toàn** trên Production

**Kiểm Tra Guard**:
```python
# Pseudocode
if not sql.strip().upper().startswith("SELECT"):
    raise RuntimeError("Production chỉ cho phép SELECT")
```

(nguồn: agents.md - 3.1)

## 7. Cơ Chế Giám Sát & Log

### Log Format
```
2026-05-22 14:30:45.123  [hanoi] [DimensionLoader] Nạp DMBenhNhan...
2026-05-22 14:30:47.456  [hanoi] [DimensionLoader] [START] MERGE ODS -> Datamart cho DimBenhNhan...
2026-05-22 14:30:48.789  [hanoi] [DimensionLoader] [SUCCESS] Hoàn thành MERGE DimBenhNhan
```

**Quy Tắc**:
- Timestamp đến mili-giây (`.mmm`)
- `flush=True` để in ngay tức thời
- Cặp `[START]` / `[SUCCESS]` cho mỗi dimension

(nguồn: GEM_AUTO_PIPELINE.md)

### Tiến Độ ODBC
- Log số dòng nạp mỗi batch
- Log tổng số dòng sau mỗi batch
- Giúp giám sát tốc độ và phát hiện hang

(nguồn: PROJECT_CHRONICLE.md - ADR-12)

---

## Các Trang Liên Quan

- [[orchestrator-dong-bo]] - Điều phối Dimension
- [[fact-loader]] - Nạp Fact incremental
- [[kien-truc-da-tang]] - Kiến trúc 3 chặng
- [[loi-va-khac-phuc]] - Xử lý lỗi ODBC/IDENTITY
