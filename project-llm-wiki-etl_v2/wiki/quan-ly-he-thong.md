# Quản Lý Hệ Thống - Cấu Hình & Facility

**Tóm tắt**: Hệ thống ETL được quản trị tập trung qua file `config/tables.yaml`. File này chứa cấu hình vận hành, danh sách facility, tham số chunk, và ma trận các bảng Dimension/Fact cần xử lý.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_AUTO_PIPELINE.md, GEM_CODE_MAP.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Vai Trò YAML vs .env

### YAML: config/tables.yaml
- **Mục tiêu**: Cấu hình **lôgic vận hành** (facility, bảng, thứ tự chạy, chunk size)
- **Nội dung**: Ma trận multi-tenant, danh sách dimension/fact, thứ tự ưu tiên
- **Tần suất**: Thay đổi khi thêm facility, bảng mới, hoặc điều chỉnh strategy

(nguồn: GEM_AUTO_PIPELINE.md)

### .env File
- **Mục tiêu**: Lưu trữ **thông tin nhạy cảm** (connection strings, credentials)
- **Nội dung**: `PROD_CONNECTION_HANOI`, `DATAMART_CONNECTION_STRING`, ...
- **Tần suất**: Ít thay đổi, chỉ khi DB server/port/credentials thay đổi

(nguồn: GEM_AUTO_PIPELINE.md)

**Quy Tắc**: YAML là nguồn cấu hình chính, .env chỉ giữ chuỗi kết nối.

(nguồn: GEM_AUTO_PIPELINE.md)

## 2. Cấu Trúc Tổng Quan config/tables.yaml

```yaml
etl_settings:
  odbc_chunk_size: 1000
  active_facilities:
    - hanoi
    - hcm

facilities:
  hanoi:
    co_so_key: 1
    nguon_du_lieu_key: 10
    ma_co_so: "HN"
    staging_schema: "hanoi_hisnano_v2"
  hcm:
    co_so_key: 2
    nguon_du_lieu_key: 20
    ma_co_so: "HCM"
    staging_schema: "hcm_hisnano_v2"

dimension_tables:
  - name: "DimBenhNhan"
    source_table: "DMBenhNhan"
    merge_script: "DimBenhNhan_merge.sql"
  - ...

incremental_tables:
  - name: "ThuPhiDichVu"
    source_table: "ThuPhiDichVu"
    date_column: "NgayDenKham"
    lookback_days: 3
    selected_columns:
      - NgayDenKham
      - MaBenhNhan
      - ...
    merge_script: "merge_fact_thuphichvu_3in1.sql"
  - ...
```

(nguồn: GEM_AUTO_PIPELINE.md)

## 3. Khối etl_settings

```yaml
etl_settings:
  odbc_chunk_size: 1000           # Số dòng/batch khi nạp ODBC
  active_facilities:              # Facility mặc định chạy
    - hanoi
    - hcm
    - halong
    - haiphong
```

### odbc_chunk_size

**Ý Nghĩa**: Kích thước lô (batch) khi nạp dữ liệu từ Production qua ODBC.

**Ảnh Hưởng**:
- **Nhỏ (100-500)**: Ít tiêu tốn RAM, chậm hơn, nhiều round-trip DB
- **Trung (1000-2000)**: Cân bằng RAM & latency (khuyến cáo)
- **Lớn (>5000)**: Nhanh nhưng dễ MemoryError với cột MAX

**Mặc Định**: `1000` (phù hợp với dữ liệu y tế chứa NVARCHAR(MAX))

(nguồn: GEM_AUTO_PIPELINE.md)

### active_facilities

**Ý Nghĩa**: Danh sách facility chạy mặc định khi không truyền tham số cụ thể.

**Ví Dụ**:
```yaml
active_facilities:
  - hanoi
  - hcm
```

Orchestrator sẽ chạy `hanoi` rồi `hcm` theo thứ tự.

**Override**: Tham số hàm `orchestrator.run(target_facilities=['hanoi'])` sẽ thay thế.

(nguồn: GEM_AUTO_PIPELINE.md)

## 4. Khối facilities - Ma Trận Facility

```yaml
facilities:
  hanoi:
    co_so_key: 1                           # INT khóa facility
    nguon_du_lieu_key: 10                  # INT khóa nguồn
    ma_co_so: "HN"                         # Mã code (HN, HCM, ...)
    staging_schema: "hanoi_hisnano_v2"     # ODS schema name
  
  hcm:
    co_so_key: 2
    nguon_du_lieu_key: 20
    ma_co_so: "HCM"
    staging_schema: "hcm_hisnano_v2"
```

### co_so_key
- **Kiểu**: INT (primary key trong ODS/Datamart)
- **Mục Đích**: Định danh cơ sở duy nhất trong hệ thống
- **Ví Dụ**: 1 = Hà Nội, 2 = TP.HCM
- **Dùng Cho**: [[dimension-loader]] và [[fact-loader]] tiêm vào mỗi row

(nguồn: GEM_AUTO_PIPELINE.md)

### nguon_du_lieu_key
- **Kiểu**: INT
- **Mục Đích**: Định danh nguồn dữ liệu (có thể có multiple sources per facility)
- **Ví Dụ**: 10 = Production HIS, 11 = Import CSV, ...
- **Dùng Cho**: Hard delete cô lập, MERGE condition, audit trail

(nguồn: GEM_AUTO_PIPELINE.md)

### ma_co_so
- **Kiểu**: VARCHAR(50)
- **Mục Đích**: Mã cơ sở dạng text (dễ đọc trong report)
- **Ví Dụ**: "HN" = Hà Nội, "HCM" = TP.HCM
- **Dùng Cho**: Query báo cáo (WHERE MaCoSo = 'HN'), user interface

(nguồn: GEM_AUTO_PIPELINE.md)

### staging_schema
- **Kiểu**: VARCHAR (tên schema SQL Server)
- **Mục Đích**: Tên schema ODS của facility này
- **Ví Dụ**: "hanoi_hisnano_v2", "hcm_hisnano_v2"
- **Dùng Cho**: Render MERGE template placeholder `{staging_schema}`

(nguồn: GEM_AUTO_PIPELINE.md)

## 5. Khối dimension_tables

```yaml
dimension_tables:
  - name: "DimBenhNhan"
    source_table: "DMBenhNhan"
    merge_script: "DimBenhNhan_merge.sql"
    
  - name: "DimBenh"
    source_table: "DMBenh"
    merge_script: "DimBenh_merge.sql"
    
  - name: "DimLoaiGoiDichVu"
    source_table: "LoaiGoiDichVuNT"
    merge_script: "DimLoaiGoiDichVu_merge.sql"
    
  - name: "DimDichVu"
    source_tables:       # Exception: 3 bảng
      - DMLoaiDichVu
      - DMDichVu
      - DMDichVuChiTiet
    merge_script: "dim_dich_vu_merge.sql"
```

### Cấu Trúc

| Trường | Ý Nghĩa |
|---|---|
| `name` | Tên dimension (DimBenhNhan, ...) |
| `source_table` hoặc `source_tables` | Bảng nguồn từ Production (1 hoặc 3 bảng) |
| `merge_script` | File SQL template cho MERGE Datamart |

### Đặc Điểm DimDichVu
- Exception duy nhất: 3 bảng Production (`DMLoaiDichVu`, `DMDichVu`, `DMDichVuChiTiet`)
- Bắt buộc nạp 3 bảng tuần tự trước khi MERGE Datamart
- Không được nạp song song

(nguồn: PROJECT_CHRONICLE.md - ADR-10)

## 6. Khối incremental_tables

```yaml
incremental_tables:
  - name: "ThuPhiDichVu"
    source_table: "ThuPhiDichVu"
    date_column: "NgayDenKham"
    lookback_days: 3
    selected_columns:
      - NgayDenKham
      - MaBenhNhan
      - TongTien
      - TongGiam
      - TongTang
    merge_script: "merge_fact_thuphichvu_3in1.sql"
  
  - name: "ThuPhiBaoHiem"
    source_table: "ThuPhiBaoHiem"
    date_column: "NgayDenKham"
    lookback_days: 3
    selected_columns:
      - NgayDenKham
      - MaBenhNhan
      - TongTien
      - TienChenhLech
    merge_script: "merge_fact_thuphichvu_3in1.sql"
```

### Cấu Trúc

| Trường | Ý Nghĩa |
|---|---|
| `name` | Tên bảng fact (ThuPhiDichVu, ...) |
| `source_table` | Bảng nguồn từ Production |
| `date_column` | Cột ngày để filter D-3 (NgayDenKham, NgayThu, ...) |
| `lookback_days` | Số ngày nhìn lại (thường 3) |
| `selected_columns` | **Whitelist cột** từ Production (chỉ lấy cái cần) |
| `merge_script` | File SQL template cho MERGE ODS & Datamart |

(nguồn: PROJECT_CHRONICLE.md - ADR-31)

### selected_columns - Whitelist

**Ý Nghĩa**: Danh sách rõ ràng các cột cần lấy từ Production.

**Tại Sao Whitelist**:
- ✅ Giảm tải RAM (chỉ lấy cột cần)
- ✅ Tránh cột bẩn (TEXT, NVARCHAR(MAX) chứa binary)
- ✅ Kiểm soát rõ ràng
- ❌ Không tự động quét INFORMATION_SCHEMA

**Ví Dụ**:
```yaml
selected_columns:
  - NgayDenKham          # Cột ngày để filter
  - MaBenhNhan           # FK dimension
  - TongTien             # Cột tài chính chính
  - TongGiam             # Công thức fallback
  - TongTang
  # Không lấy cột: Notes, Description (nếu là TEXT)
```

(nguồn: PROJECT_CHRONICLE.md - ADR-31)

## 7. Cách Mở Rộng YAML

### Thêm Facility Mới

```yaml
facilities:
  halong:
    co_so_key: 3
    nguon_du_lieu_key: 30
    ma_co_so: "HL"
    staging_schema: "halong_hisnano_v2"
```

**Bước Sau**:
1. Tạo schema `halong_hisnano_v2` trên SQL Server
2. Thêm `PROD_CONNECTION_HALONG` vào `.env`
3. Cập nhật `etl_settings.active_facilities` nếu cần chạy ngay

(nguồn: GEM_AUTO_PIPELINE.md)

### Thêm Bảng Fact Mới

```yaml
incremental_tables:
  - name: "BangMoi"
    source_table: "BangMoi"
    date_column: "NgayPhát"
    lookback_days: 3
    selected_columns:
      - NgayPhát
      - MaKey
      - GiaTri
    merge_script: "BangMoi_merge.sql"
```

**Bước Sau**:
1. Tạo SQL template `BangMoi_merge.sql` trong `src/db/templates/sql/`
2. Thêm YAML entry
3. Tạo bảng trong Global Staging + ODS + Datamart
4. Chạy orchestrator

(nguồn: PROJECT_CHRONICLE.md - ADR-21)

## 8. Đồng Bộ Whitelist Column từ Schema

### Script: sync_selected_columns_from_staging.py

```bash
python scripts/sync_selected_columns_from_staging.py --preview
python scripts/sync_selected_columns_from_staging.py --write
```

**Tác Dụng**:
- Quét schema Staging (`stg_nano_v2`) bằng `INFORMATION_SCHEMA.COLUMNS`
- Tự động sinh `selected_columns` theo thứ tự cột trong DB
- Loại bỏ cột hệ thống (`MaCoSo`, `CoSoKey`, `NguonDuLieuKey`)
- In preview hoặc ghi trực tiếp vào YAML

**Ưu Điểm**:
- Tiết kiệm công ghi tay
- Đồng bộ với cấu trúc DB thực tế

(nguồn: PROJECT_CHRONICLE.md - ADR-36)

---

## Các Trang Liên Quan

- [[orchestrator-dong-bo]] - Đọc cấu hình YAML
- [[dimension-loader]] - Sử dụng YAML facility & chunk_size
- [[fact-loader]] - Sử dụng YAML incremental_tables
- [[etl-v2-tong-quan]] - Tổng quan hệ thống
