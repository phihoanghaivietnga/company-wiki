# Lịch Sử Quyết Định Kiến Trúc (40 ADRs)

**Tóm tắt**: Tài liệu này ghi lại 40 quyết định kiến trúc (Architectural Decision Records - ADRs) từ 2026-05-09 đến 2026-05-21, theo dõi sự phát triển kỹ thuật của dự án ETL từ giai đoạn hạ tầng đến ổn định cuối cùng.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (PROJECT_CHRONICLE.md section)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Giai Đoạn Hạ Tầng (2026-05-09)

### ADR-01: Kiến Trúc Native OOP vs Client-Server API
- **Vấn đề**: Gọi API HTTP nội bộ phát sinh độ trễ mạng không cần thiết
- **Quyết định**: Gọi trực tiếp logic Python bằng `nicegui.run.io_bound` (cùng tiến trình)
- **Lợi ích**: Giảm latency, cải thiện dashboard real-time

(nguồn: PROJECT_CHRONICLE.md - 2026-05-15)

### ADR-02: DB Connection Manager Pattern
- **Vấn đề**: Kết nối DB vĩnh cửu → rò rỉ tài nguyên, treo session
- **Quyết định**: Mọi query phải đi qua context manager `get_db_context()`, nạp chuỗi động từ `.env`
- **Lợi ích**: Tránh rò rỉ tài nguyên, an toàn bộ nhớ

(nguồn: PROJECT_CHRONICLE.md - 2026-05-15)

### ADR-03: Semaphore Bảo Vệ Production
- **Vấn đề**: ETL bắn đồng loạt query quá tải Production
- **Quyết định**: Bọc `asyncio.gather()` dưới `MAX_CONCURRENT_CONNECTIONS` semaphore
- **Lợi ích**: Bảo vệ Production, duy trì ổn định

(nguồn: PROJECT_CHRONICLE.md - 2026-05-15)

### ADR-04: State Isolation cho UI
- **Vấn đề**: Trạng thái UI rò rỉ giữa tab/user khác
- **Quyết định**: Wrapper function dưới `@ui.page` khởi tạo instance class mới mỗi request
- **Lợi ích**: Vòng đời độc lập per phiên, an toàn dữ liệu

(nguồn: PROJECT_CHRONICLE.md - 2026-05-15)

### ADR-05: Multi-Grid All-in-One Dashboard
- **Vấn đề**: Dropdown chọn từng bảng → cậy thả so sánh cấu trúc
- **Quyết định**: Hiển thị toàn bộ lưới đối chiếu trên một màn hình (Dim trước, Fact sau)
- **Lợi ích**: Tách bạch bản chất dữ liệu, giữ tính đúng đắn cột động

(nguồn: PROJECT_CHRONICLE.md - 2026-05-15)

## 2. Giai Đoạn Khung ETL Tuần Tự (2026-05-18)

### ADR-06: SyncOrchestrator Chạy Tuần Tự
- **Vấn đề**: Cần điều phối toàn bộ facility theo thứ tự
- **Quyết định**: Tạo `SyncOrchestrator` chạy tuần tự, hỗ trợ `ACTIVE_FACILITIES` filter
- **Lợi ích**: Không hardcode, vận hành linh hoạt per facility

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-06)

### ADR-07: Dimension 2-Hop
- **Vấn đề**: Danh mục cần full-load để đảm bảo tính nhất quán
- **Quyết định**: Prod → ODS (TRUNCATE + nạp) → Datamart (MERGE)
- **Lợi ích**: Tách rõ landing từ ODS, bảo vệ lịch sử

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-07)

### ADR-08: Fact 3-Hop với Hard Delete D-3
- **Vấn đề**: Phát sinh cần incremental, nhưng phải bảo vệ lịch sử
- **Quyết định**: Prod → Landing → ODS → Datamart, hard delete chỉ trong D-3
- **Lợi ích**: Bắt dữ liệu muộn, giữ lịch sử dài

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-08)

### ADR-09: Công Thức Tài Chính 3-in-1 & Seed Key
- **Vấn đề**: 3 bảng doanh thu có công thức riêng, fact đến trước dimension
- **Quyết định**: Hardcode công thức, fallback seed key `-1` cho early facts
- **Lợi ích**: Tính nhất quán tài chính, không mất bản ghi

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-09)

### ADR-10: Chuẩn Hóa FULL_LOAD 4 Dimension
- **Vấn đề**: Quá nhiều dimension, dễ xóa nhầm danh mục incremental
- **Quyết định**: Chỉ hỗ trợ 4 dimension chính, gỡ DimLuotKham khỏi DimensionLoader
- **Lợi ích**: An toàn, tránh sai lệch kiến trúc

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-10)

### ADR-11: Quân Luật Kết Nối DB
- **Vấn đề**: Cần bảo vệ Production khỏi lệnh thay đổi
- **Quyết định**: Production: CHỈ ĐỌC; Staging/Datamart: FULL WRITE
- **Lợi ích**: Bảo vệ dữ liệu tác nghiệp gốc

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-11)

### ADR-12: Giám Sát Real-time FULL_LOAD
- **Vấn đề**: Log bị nuốt khi nạp bảng lớn (DimBenhNhan)
- **Quyết định**: Dùng `subprocess.Popen` stream log thực thời thay vì `.run()`
- **Lợi ích**: Thấy tiến độ, phát hiện hang sớm

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-12)

### ADR-13: Hotfix Mất Đồng Bộ `_log()` Chữ Ký
- **Vấn đề**: `DimensionLoader._log()` override bị lỗi TypeError
- **Quyết định**: Thêm `**kwargs` để chứa tham số không sử dụng từ `BaseLoader`
- **Lợi ích**: Tương thích đa hình, không gãy lúc runtime

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-13)

### ADR-14: Hotfix Deadlock TRUNCATE + BCP
- **Vấn đề**: `pyodbc` giữ lock khi `BCP` subprocess không thể ghi
- **Quyết định**: Thêm `connection.commit()` ngay sau TRUNCATE để giải phóng lock
- **Lợi ích**: Tránh treo pipeline

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-14)

### ADR-15: Hotfix Log BCP Security & Cast Error
- **Vấn đề**: Log in toàn bộ command BCP (lộ password), BCP thiếu `-k` flag
- **Quyết định**: Ẩn command, thêm `-w -t "<|>" -k` cho BCP
- **Lợi ích**: Bảo mật + ổn định ép kiểu

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-15)

### ADR-16: Hotfix Cú Pháp BCP & Che Giấu Traceback
- **Vấn đề**: Exception vẫn lộ command, tham số BCP không chuẩn
- **Quyết định**: Chuẩn hóa list tham số BCP, raise error generic không kèm command
- **Lợi ích**: Bảo mật, ổn định command execution

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-16)

### ADR-17: Khai Tử BCP CLI, Chuyển ODBC
- **Vấn đề**: Dữ liệu HIS chứa ký tự ẩn (`\n`, `\r`, `\t`) vỡ file BCP
- **Quyết định**: Loại bỏ BCP file, nạp trực tiếp bằng `pyodbc.executemany()`
- **Lợi ích**: Bảo toàn dữ liệu text bẩn, an toàn ép kiểu

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-17)

### ADR-18: Hotfix MemoryError với Cột MAX
- **Vấn đề**: `fast_executemany=True` + cột MAX → MemoryError
- **Quyết định**: Tắt vĩnh viễn `fast_executemany`, hạ batch size 1000 rows
- **Lợi ích**: Ổn định bộ nhớ tuyệt đối (chấp nhận chậm hơn)

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-18)

## 3. Giai Đoạn YAML & Multi-tenant (2026-05-19)

### ADR-19: Ma Trận Vận Hành Sang config/tables.yaml
- **Vấn đề**: Cấu hình `.env` rời rạc, khó mở rộng multi-facility
- **Quyết định**: Chuyển sang YAML: `etl_settings`, `facilities`, `dimension_tables`, `incremental_tables`
- **Lợi ích**: Tập trung, dễ mở rộng

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-19)

### ADR-20: Tenant Injection
- **Vấn đề**: ODS chứa dữ liệu multi-tenant nhưng Production không có định danh
- **Quyết định**: Tiêm 3 cột (`NguonDuLieuKey`, `CoSoKey`, `MaCoSo`) từ YAML vào mỗi row
- **Lợi ích**: Multi-tenant isolation, cô lập per facility

(nguồn: PROJECT_CHRONICLE.md - 2026-05-18 - ADR-20)

### ADR-21: Tách Extractor & Incremental Động
- **Vấn đề**: Fact incremental hardcode danh sách bảng, khó mở rộng
- **Quyết định**: Tạo `BaseExtractor`, đọc `incremental_tables` từ YAML, dynamic SELECT
- **Lợi ích**: Không hardcode, cấu hình trực tiếp qua YAML

(nguồn: PROJECT_CHRONICLE.md - 2026-05-19 - ADR-21)

## 4. Giai Đoạn Sửa Lỗi BCP (2026-05-20)

### ADR-22: Hotfix Lock Tầng Landing BCP
- **Vấn đề**: TRUNCATE Landing chưa commit → BCP IN chờ lock
- **Quyết định**: `connection.commit()` ngay sau TRUNCATE Landing
- **Lợi ích**: Tránh treo incremental pipeline

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-22)

### ADR-23: Masking NULL Cho Cột loại trừ
- **Vấn đề**: Dynamic SELECT loại cột datatype → lệch schema BCP 22005
- **Quyết định**: Không loại cột, mask bằng `CAST(NULL AS VARCHAR(1))`
- **Lợi ích**: Giữ hình học cột 1:1, chống lệch

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-23)

### ADR-24: Sanitize Log ETL
- **Vấn đề**: Log in query/command rò rỉ thông tin nhạy cảm
- **Quyết định**: Log generic message, ẩn connection string/PWD/token
- **Lợi ích**: Bảo mật vận hành

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-24)

### ADR-25: Guard Doanh Thu SMI-3
- **Vấn đề**: Cần đảm bảo công thức fallback tại MERGE template
- **Quyết định**: Bổ sung `validate_sql_revenue_rules()` regex check
- **Lợi ích**: Bảo vệ logic tài chính khỏi sửa sai template

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-25)

### ADR-26: Unicode Masking NVARCHAR(MAX)
- **Vấn đề**: Masking VARCHAR(1) chưa tương thích 100% BCP -w
- **Quyết định**: Dùng `CAST(NULL AS NVARCHAR(MAX))` cho mọi cột exclude
- **Lợi ích**: Tuyệt đối tương thích Unicode wide

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-26)

### ADR-27: Ghi Nợ Kỹ Thuật Revenue Check
- **Vấn đề**: Validate SQL Fallback Doanh Thu bị gỡ theo chỉ thị
- **Quyết định**: Ghi nhận rủi ro: ETL hoàn toàn tin tưởng SQL template
- **Lợi ích**: Rõ trách nhiệm, chuẩn bị contingency

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-27)

## 5. Giai Đoạn Chuẩn Hóa BCP v4-v7 (2026-05-20)

### ADR-28: Parse Connection BCP Regex
- **Vấn đề**: BCP OUT/IN thiếu tham số kết nối, lỗi source/dest mix
- **Quyết định**: `parse_connection_string()` dùng Regex, tách `-S -d -U -P` rõ ràng
- **Lợi ích**: Robust BCP command building

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-28)

### ADR-29: Transaction Biên Tầng 1 Chống Deadlock
- **Vấn đề**: Giữ connection xuyên suốt TRUNCATE + BCP → treo
- **Quyết định**: Chuỗi cứng: BCP OUT → Connection A TRUNCATE+commit+close → BCP IN → Connection B MERGE
- **Lợi ích**: Tách rõ session, chống deadlock

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-29)

### ADR-30: Strip Trailing Spaces Metadata
- **Vấn đề**: pyodbc metadata có khoảng trắng ở đầu/cuối → lệch nhận diện type
- **Quyết định**: `.strip()` column_name, data_type từ INFORMATION_SCHEMA
- **Lợi ích**: Chuẩn hóa metadata

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-30)

### ADR-31: Khai Tử exclude_datatypes, Chuyển Whitelist
- **Vấn đề**: Black-list datatype phức tạp, khó kiểm soát hình học cột
- **Quyết định**: Mỗi bảng incremental khai báo `selected_columns` rõ ràng
- **Lợi ích**: Kiểm soát, giảm tải RAM từ đầu

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-31)

### ADR-32: Dynamic SELECT từ Whitelist + Enrichment
- **Vấn đề**: Cần ghép tenant key vào SELECT khi nạp
- **Quyết định**: BUILD SELECT từ `selected_columns`, ghép 3 cột (`CoSoKey`, `NguonDuLieuKey`, `MaCoSo`)
- **Lợi ích**: Giữ 1:1 hình học cột, tenant vẫn có sẵn

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-32)

### ADR-33: Cô Lập Transaction Biên Tầng 1
- **Vấn đề**: TRUNCATE + BCP phối hợp yêu cầu lock release chặt chẽ
- **Quyết định**: Connection A TRUNCATE+commit+close riêng, BCP IN subprocess riêng, Connection B MERGE riêng
- **Lợi ích**: 3 session độc lập, không lock chờ

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-33)

### ADR-34: Hủy BCP cho Incremental
- **Vấn đề**: BCP phát sinh nhiều điểm nhạy cảm (schema, lock, cast)
- **Quyết định**: Loại bỏ hoàn toàn BCP incremental, quay về PyODBC
- **Lợi ích**: Ổn định implicit casting, giảm phức tạp

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-34)

### ADR-35: Tầng 1 PyODBC Chunking
- **Vấn đề**: Nạp PyODBC cần chuẩn hóa batch + INSERT động
- **Quyết định**: `fetchmany(batch_size)` + `executemany()`, INSERT tường minh theo `selected_columns`
- **Lợi ích**: Ổn định, giảm tải RAM

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-35)

### ADR-36: Script Sync Whitelist từ Staging
- **Vấn đề**: Ghi tay `selected_columns` YAML mất công, dễ nhầm
- **Quyết định**: Tạo `sync_selected_columns_from_staging.py` quét INFORMATION_SCHEMA
- **Lợi ích**: Tự động sinh YAML, giảm manual work

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-36)

## 6. Giai Đoạn IDENTITY_INSERT & ổn định (2026-05-20-21)

### ADR-37: IDENTITY_INSERT Động trong PyODBC
- **Vấn đề**: Bảng Landing có cột IDENTITY → lỗi SQL 544 khi insert
- **Quyết định**: Kiểm tra `TableHasIdentity`, bật/tắt `SET IDENTITY_INSERT` trong try/finally
- **Lợi ích**: Tự động detect, không lỗi ID violation

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-37)

### ADR-38: Tắt fast_executemany Vĩnh Viễn
- **Vấn đề**: `fast_executemany=True` với cột MAX → MemoryError persistent
- **Quyết định**: Tắt hoàn toàn, accept Row-by-Row ngầm ODBC, chấp nhận chậm hơn
- **Lợi ích**: An toàn bộ nhớ tuyệt đối

(nguồn: PROJECT_CHRONICLE.md - 2026-05-20 - ADR-38)

### ADR-39: Manual Runner Điều Phối Động
- **Vấn đề**: Manual Runner dùng `GenericTableLoader`, không chạy full 3-hop
- **Quyết định**: Loại bỏ `GenericTableLoader`, điều phối động theo bảng chọn
- **Lợi ích**: UI chạy full pipeline (Prod → ODS → DM)

(nguồn: PROJECT_CHRONICLE.md - 2026-05-21 - ADR-39)

### ADR-40: Gom 3 Bảng Doanh Thu Thành Cụm
- **Vấn đề**: 3 bảng doanh thu riêng lẻ trên UI → user nhầm lẫn, thiếu đồng bộ
- **Quyết định**: Gom thành "ThuPhiDichVu" (chỉ 1 nút), tự động nạp 3 bảng tuần tự
- **Lợi ích**: Tính nhất quán tài chính, UX rõ ràng

(nguồn: PROJECT_CHRONICLE.md - 2026-05-21)

---

## Tóm Tắt Hành Trình Kỹ Thuật

| Giai Đoạn | Thời Gian | Điểm Cao | Điểm Thách Thức |
|---|---|---|---|
| **Hạ Tầng** | 5/15 | OOP, State Isolation | Dashboard complexity |
| **ETL Tuần Tự** | 5/18 | 2-hop/3-hop architecture | BCP lock/deadlock |
| **YAML & Tenant** | 5/19 | Dynamic config, isolation | Flexible schema |
| **BCP → ODBC** | 5/20 | Security, stability | Memory management |
| **IDENTITY & UI** | 5/21 | Production-ready | Full-stack integration |

**Bài Học**:
- Từng ADR là kết quả học tập từ vấn đề thực tế
- Không có "solution tối ưu lần đầu" - cần iteration
- Security & stability (ADR-11, ADR-38) là ưu tiên hàng đầu

---

## Các Trang Liên Quan

- [[loi-va-khac-phuc]] - Chi tiết vấn đề & giải pháp từng ADR
- [[kien-truc-da-tang]] - Kiến trúc cuối cùng
- [[orchestrator-dong-bo]] - Triển khai ADR-06
- [[fact-loader]] - Triển khai ADR-08, ADR-34
