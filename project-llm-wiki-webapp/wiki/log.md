# Nhật ký thao tác

## 01/07/2026

- **Sync Partner Tracker** — Đồng bộ file Excel `issue_webapp_20260701.xlsx` (Luồng 11)
- File snapshot lưu tại: `partner-tracker/snapshots/issue_webapp_20260701.xlsx`
- Đã tạo `partner-tracker/tracker.md` từ 3 sheet: Check (38 issues), Plan 05 (11 kế hoạch), Thông tin nhân sự ISOFH
- Kết quả: 35 issues đang mở (open), 3 issues đã đóng (resolved: ISSUE-007, ISSUE-021, ISSUE-025)
- 5 issues chỉ có ảnh, không có mô tả văn bản: ISSUE-029, ISSUE-030, ISSUE-032, ISSUE-038
- 1 conflict tiềm ẩn: ISSUE-010 (HIS không bắt buộc CCCD nhưng WebApp bắt buộc CCCD)
- Đã liên kết các issue đến trang wiki: [[dat-lich-kham]], [[dong-bo-du-lieu-his]], [[xac-thuc-bao-mat]], [[web-vendor]], [[zalo-sms]], [[thong-bao-tin-tuc]], [[ho-so-suc-khoe]], [[luong-dang-ky-kham]]

## 23/06/2026

- **Cập nhật** tài liệu `20260619_luong_dong_bo_du_lieu_webapp_his.md`
- Đã bổ sung vào `dong-bo-du-lieu-his.md`:
  - Phần "Điều kiện định danh bệnh nhân" với chi tiết 6 trường thông tin
  - Phần "Cơ chế tra cứu và đồng bộ tự động" mô tả quy trình 2 bước
  - Mục mới "Từ tài liệu mới" trong các vấn đề cần làm rõ
- Chuyển dữ liệu từ `raw/do` sang `raw/done`

## 13/06/2026

- **Ingest lần đầu** bộ tài liệu WebApp ISOFH - BV Mắt Quốc tế Việt Nga
- Nguồn: 16 file trong `raw/do` (manifest + 15 tài liệu nội dung)
- Đã tạo các trang wiki:
  - `index.md` - Mục lục
  - `tong-quan-he-thong.md` - Tổng quan hệ thống
  - `kien-truc-he-thong.md` - Kiến trúc hệ thống
  - `dat-lich-kham.md` - Đặt lịch khám
  - `ho-so-suc-khoe.md` - Hồ sơ sức khỏe
  - `dong-bo-du-lieu-his.md` - Đồng bộ dữ liệu HIS
  - `xac-thuc-bao-mat.md` - Xác thực và bảo mật
  - `web-vendor.md` - Web Vendor tổng quan
  - `huong-dan-su-dung-web-vendor.md` - HDSD Web Vendor
  - `huong-dan-su-dung-web-tra-cuu.md` - HDSD Web tra cứu
  - `zalo-sms.md` - Zalo SMS
  - `thong-bao-tin-tuc.md` - Thông báo và tin tức
  - `huong-dan-tao-du-lieu-test.md` - Hướng dẫn tạo dữ liệu test
  - `luong-dang-ky-kham.md` - Luồng đăng ký khám
  - `cac-van-de-ton-dong.md` - Các vấn đề tồn đọng
- Chuyển dữ liệu từ `raw/do` sang `raw/done`