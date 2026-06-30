# Orchestrator - Điều Phối Tuần Tự

**Tóm tắt**: Module `SyncOrchestrator` điều phối luồng ETL theo chế độ tuần tự đơn cơ sở (một facility tại một thời điểm), hỗ trợ selective sync để chọn lọc cơ sở chạy mà không hardcode toàn bộ hệ thống.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_AUTO_PIPELINE.md, GEM_CODE_MAP.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Mục Tiêu

- **Tuần tự an toàn**: Chạy từng facility xong rồi mới sang facility kế tiếp
- **Selective Sync**: Hỗ trợ chọn lọc cơ sở (facility) chạy thay vì hardcode toàn bộ
- **Phòng chống lỗi**: Nếu facility hiện tại gặp lỗi, dừng ngay để tránh lan lỗi

(nguồn: GEM_AUTO_PIPELINE.md)

## 2. Quy Tắc Điều Phối Bắt Buộc

### Thứ Tự Tuần Tự
```
Facility 1
├── Dimension Loader (full-load)
├── Fact Loader (incremental)
└── Dọn Landing (finally block)
    ↓
Facility 2
├── Dimension Loader
├── Fact Loader
└── Dọn Landing
    ↓
Facility 3
└── ...
```

**Quy tắc**:
1. **Tuần tự**: Không chạy song song giữa các cơ sở
2. **Hoàn tất**: Facility hiện tại phải hoàn tất toàn bộ Dimension + Fact + cleanup Landing trước khi sang cơ sở kế tiếp
3. **Dừng lỗi**: Nếu có lỗi tại một facility thì dừng luồng tuần tự để tránh lan lỗi

(nguồn: GEM_AUTO_PIPELINE.md)

## 3. Selective Sync - Chọn Lọc Cơ Sở

### Cách Chọn Cơ Sở

**Cách 1: YAML Config** (ưu tiên)
```yaml
# config/tables.yaml
etl_settings:
  active_facilities:
    - hanoi
    - hcm
```

**Cách 2: Tham Số Hàm**
```python
orchestrator = SyncOrchestrator(...)
orchestrator.run(target_facilities=['hanoi', 'hcm'])
```

**Cách 3: Chạy Toàn Bộ**
```python
# Không truyền cơ sở cụ thể = chạy tất cả facility định nghĩa
orchestrator.run()  # hoặc
orchestrator.run(target_facilities=['ALL'])
```

**Ưu tiên**: Tham số hàm `target_facilities` > YAML config > chạy toàn bộ

### Quản Lý Kết Nối

- Chỉ khởi tạo connection cho facility được chọn
- Facility ngoài scope **không được** tạo connection để tiết kiệm tài nguyên
- Mỗi facility chạy xong sẽ đóng connection trước khi sang facility kế tiếp

(nguồn: GEM_AUTO_PIPELINE.md)

## 4. Cấu Hình YAML Tập Trung

**File chuẩn**: `config/tables.yaml`

### Khối `etl_settings`
```yaml
etl_settings:
  odbc_chunk_size: 1000           # Kích thước lô nạp ODBC (rows/batch)
  active_facilities:              # Danh sách facility chạy mặc định
    - hanoi
    - hcm
```

**Ý nghĩa**:
- `odbc_chunk_size`: Số dòng được nạp mỗi lô (ảnh hưởng đến RAM, latency)
- `active_facilities`: Danh sách facility mặc định nếu không truyền tham số

(nguồn: GEM_AUTO_PIPELINE.md)

### Khối `facilities`
```yaml
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
```

**Ý nghĩa**:
- `co_so_key`: Integer định danh cơ sở trong ODS
- `nguon_du_lieu_key`: Integer định danh nguồn dữ liệu
- `ma_co_so`: String mã cơ sở (ví dụ: "HN", "HCM")
- `staging_schema`: Tên schema ODS của facility

(nguồn: GEM_AUTO_PIPELINE.md)

### Quy Tắc YAML Chung

- **YAML là nguồn cấu hình chính** cho matrix multi-tenant
- `.env` **chỉ giữ chuỗi kết nối** (connection strings)
- Mọi thay đổi cơ sở / chunk size / thứ tự facility **phải cập nhật YAML**

(nguồn: GEM_AUTO_PIPELINE.md)

## 5. Input Connection Chuẩn

**Kết Nối Datamart** (đích chung):
```
DATAMART_CONNECTION_STRING=Driver={ODBC Driver 17 for SQL Server}; ...
```

**Kết Nối Production theo Facility** (nguồn):
```
PROD_CONNECTION_HANOI=Driver=...
PROD_CONNECTION_HCM=Driver=...
PROD_CONNECTION_HALONG=Driver=...
PROD_CONNECTION_HAIPHONG=Driver=...
```

**Ý nghĩa**:
- Mỗi facility có connection Production riêng (định danh server/database)
- Datamart chung dùng cho tất cả facility
- `.env` phải chứa tất cả các connection này

(nguồn: GEM_AUTO_PIPELINE.md)

## 6. Luồng Gọi Loader

**Trình Tự Bắt Buộc** cho từng facility:
```python
# Tương tự Python pseudocode
for facility in target_facilities:
    try:
        # Bước 1: Nạp full Dimension
        dimension_loader = DimensionLoader(facility, ...)
        dimension_loader.execute_load()
        
        # Bước 2: Nạp incremental Fact
        fact_loader = FactLoader(facility, ...)
        fact_loader.execute_load()
        
    except Exception as e:
        log.error(f"Facility {facility} failed: {e}")
        raise  # Dừng ngay, không tiếp tục facility kế tiếp
```

**Điểm Quan Trọng**:
- [[dimension-loader]] phải chạy xong trước [[fact-loader]]
- Nếu Dimension thất bại → không chạy Fact
- Nếu Fact thất bại → dừng luồng tuần tự

(nguồn: GEM_AUTO_PIPELINE.md)

## 7. Chốt Chặn Vận Hành

### Landing Cleanup
- **Đầu**: FactLoader tự động `TRUNCATE` Landing trước nạp
- **Cuối**: FactLoader tự động `TRUNCATE` Landing trong finally block
- Mục tiêu: Không để rò rỉ dữ liệu giữa các facility

(nguồn: GEM_AUTO_PIPELINE.md)

### Hard Delete Incremental
- Chỉ được xóa dữ liệu **trong cửa sổ D-3** (3 ngày gần nhất)
- Bắt buộc cô lập theo `NguonDuLieuKey` để không xóa nhầm cơ sở khác
- Giữ lịch sử ngoài 3 ngày để audit trail

(nguồn: GEM_AUTO_PIPELINE.md)

### MERGE Fact Safeguard
- Tất cả MERGE Fact lên Datamart **bắt buộc** kiểm tra `NguonDuLieuKey`
- Điều kiện `ON`: `Target.NguonDuLieuKey = Source.NguonDuLieuKey`
- Tránh xóa dữ liệu Datamart của facility khác

(nguồn: GEM_AUTO_PIPELINE.md)

## 8. Cơ Chế Giám Sát (Monitoring)

### Log Real-time

**Timestamp Format**:
```
2026-05-22 14:30:45.123  [hanoi] [DimensionLoader] [START] Nạp DimBenhNhan...
2026-05-22 14:30:47.456  [hanoi] [DimensionLoader] [SUCCESS] Hoàn thành DimBenhNhan
```

**Quy Tắc**:
- Format: `YYYY-MM-DD HH:MM:SS.mmm` (đến mili-giây)
- `flush=True` để in ngay lập tức (không buffer)
- Luồng ODBC copy log tiến độ theo tổng số dòng đã nạp từng lô

(nguồn: GEM_AUTO_PIPELINE.md)

### Trạng Thái MERGE
```
[START] Đang thực thi MERGE ODS -> Datamart cho DimBenhNhan...
[SUCCESS] Hoàn thành MERGE DimBenhNhan
```

- Log rõ ràng cặp START/SUCCESS cho từng dimension
- Giúp giám sát tiến độ qua log file

(nguồn: GEM_AUTO_PIPELINE.md)

---

## Các Trang Liên Quan

- [[dimension-loader]] - Nạp full-load Dimension
- [[fact-loader]] - Nạp incremental Fact
- [[huong-dan-auto-runner]] - Hướng dẫn vận hành Auto
- [[kien-truc-da-tang]] - Kiến trúc 3 chặng chung
