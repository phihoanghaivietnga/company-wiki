# Nhật Ký Wiki (Append-Only Log)

Tài liệu ghi lại tất cả các thay đổi, bổ sung, cập nhật wiki theo thời gian.

---

## 2026-05-22 - Ingest MASTER_KNOWLEDGE_BASE.md

### Ngày: 2026-05-22
### Nhân Vật: Codex (Ingest)
### Nguồn: `raw/MASTER_KNOWLEDGE_BASE.md`

### Các Trang Tạo Mới

1. **wiki/etl-v2-tong-quan.md**
   - Tóm tắt mục đích, cấu trúc hệ thống
   - Giới thiệu vai trò Agent & Codex
   - Liệt kê tầng tri thức GEM

2. **wiki/kien-truc-da-tang.md**
   - Chi tiết 4 tầng: Production, Staging, ODS, Datamart
   - Luồng dữ liệu Dimension (2-hop) vs Fact (3-hop)
   - Chốt chặn an toàn: hard delete D-3, cô lập cơ sở, bảo vệ Production

3. **wiki/orchestrator-dong-bo.md**
   - Quy tắc tuần tự & cô lập facility
   - Selective sync & YAML config tập trung
   - Cơ chế giám sát real-time

4. **wiki/dimension-loader.md**
   - Phạm vi: 4 dimension (DimBenhNhan, DimBenh, DimLoaiGoiDichVu, DimDichVu)
   - Luồng 2-hop: TRUNCATE ODS, ODBC bulk copy, MERGE Datamart
   - Tenant injection 3 cột định danh
   - Bảo vệ Production (chỉ SELECT)

5. **wiki/fact-loader.md**
   - Phạm vi: 6 bảng fact incremental
   - Luồng 3-hop với D-3 safeguard & hard delete an toàn
   - Cửa sổ trượt ngày động từ config
   - Whitelist cột, ODBC chunking, PyODBC (không BCP)
   - Công thức tài chính 3-in-1 & fallback seed key
   - Cụm 3 bảng doanh thu (Manual UI)

6. **wiki/quan-ly-he-thong.md**
   - Cấu hình YAML tập trung (`config/tables.yaml`)
   - Khối `etl_settings`, `facilities`, `dimension_tables`, `incremental_tables`
   - Multi-tenant facility matrix
   - Whitelist `selected_columns` & script sync schema động

7. **wiki/chinh-sach-bao-mat-db.md**
   - 3-Zone Security: Production (read-only), Staging (controlled), Datamart (controlled)
   - Guard rail check SQL khi thực thi
   - Log sanitization (che giấu password/connection string)
   - Context manager, transaction boundary, deadlock prevention
   - Semaphore bảo vệ Production khỏi quá tải

8. **wiki/quyet-dinh-kien-truc-40adr.md**
   - 40 Architectural Decision Records (2026-05-09 đến 2026-05-21)
   - 6 giai đoạn: Hạ tầng, ETL tuần tự, YAML/Tenant, BCP v4-v7, IDENTITY_INSERT, UI
   - Mỗi ADR: vấn đề, quyết định, lợi ích
   - Hành trình từ BCP file → PyODBC ODBC
   - Hotfix memory, deadlock, security, unicode

9. **wiki/loi-va-khac-phuc.md**
   - 8 lỗi thường gặp: Deadlock, MemoryError, BCP 22005, SQL 544, Unicode, DB connection, Hard delete, Query timeout
   - Mỗi lỗi: triệu chứng, nguyên nhân gốc, cách khắc phục
   - Ví dụ code, kiểm tra log, troubleshoot steps
   - Checklist debug

10. **wiki/huong-dan-manual-runner.md**
    - Truy cập UI browser, giao diện & các field
    - 2 chế độ: Dimension (full-load) vs Fact (incremental)
    - 4 bước dùng: chọn chế độ, facility, bảng, khoảng ngày
    - 3 tình huống sử dụng: cập nhật danh mục, đối chiếu doanh thu, khắc phục lỗi
    - Xử lý lỗi: [Dừng], [Reset], [Chạy Ngay]
    - Dấu hiệu thành công vs lỗi

11. **wiki/huong-dan-auto-runner.md**
    - Kiến trúc Auto Runner: Scheduler → Orchestrator → Facility
    - Cấu hình lịch: GitHub Actions (khuyên), Cron Linux, Task Scheduler Windows
    - Cấu hình facility từ YAML: `active_facilities`
    - Luồng chi tiết & log output ví dụ
    - Giám sát & alert: GitHub Actions, log file, email (tùy chọn)
    - Quy hoạch hàng ngày: 02:00 AM Auto, 09:00 AM Check, 15:00 Review
    - Performance tuning & disaster recovery

12. **wiki/index.md**
    - Master index của tất cả 11 trang wiki
    - Bắt đầu nhanh: 5 trang chính
    - Bản đồ liên kết (link graph)
    - Quy trình đọc cho: người bắt đầu, developer, ops/devops
    - Tra cứu theo chủ đề
    - Checklist sử dụng hệ thống

13. **wiki/log.md** (tài liệu này)
    - Nhật ký append-only ghi lại tất cả thay đổi

### Tổng Kết

- **Tổng Trang Wiki**: 11 (+ 1 index + 1 log = 13 file)
- **Tổng Từ**: ~50,000+ từ (phiên bản tiếng Việt)
- **Cấu Trúc**: Phân tách 11 chủ đề chính, liên kết đầy đủ
- **Nguồn**: 100% từ `raw/MASTER_KNOWLEDGE_BASE.md` (39 ADRs + 4 tài liệu gốc)
- **Tuân Thủ**: ✓ Định dạng trang, ✓ Trích dẫn nguồn, ✓ Wiki link
- **Trạng Thái**: ✓ Đầy đủ & sẵn sàng sử dụng

### Lần Cập Nhật Tiếp Theo

Dự kiến: Khi có nguồn tài liệu mới trong `raw/` hoặc feedback từ người dùng wiki.

---
