# Web Vendor - Tổng quan các module quản trị

**Tóm tắt**: Hệ thống Web Vendor dành cho nhân viên bệnh viện quản trị các tính năng của Web/App như quản lý tài khoản, banner, tin nhắn SMS, câu hỏi người dùng, nội dung website và lịch làm việc bác sĩ.

**Nguồn**: 07_hdsd_web_vendor_cntt.md, 04_plhd_app_03_03.md

**Cập nhật lần cuối**: 13/06/2026

---

## Các module chính

| Module | Chức năng |
|--------|-----------|
| Quản lý tài khoản | Khóa/mở khóa, reset mật khẩu mặc định về 123456 (nguồn: 07_hdsd_web_vendor_cntt.md) |
| Cấu hình banner app | Thêm ảnh banner, cấu hình loại màn hình đích khi nhấn vào banner |
| Quản lý tin nhắn SMS | Lưu trữ tất cả tin nhắn đã gửi, trạng thái từ đối tác SMS |
| Quản lý câu hỏi | Lưu trữ câu hỏi từ người dùng, CSYT trả lời (một lần), đánh dấu ảnh nhạy cảm |
| Quản lý website | Trang nội dung, bài viết, menu, thiết lập giao diện |
| Quản lý lịch làm việc | Thiết lập lịch bác sĩ, import Excel, lặp lịch theo tuần |
| Thiết lập | Ngôn ngữ tiếng Anh/tiếng Việt |

## Các rule nghiệp vụ nổi bật

- Không được xóa bác sĩ khi bác sĩ đã được lên lịch làm việc (nguồn: 07_hdsd_web_vendor_cntt.md)
- Chỉ đồng bộ bác sĩ được phép đặt khám online từ hệ thống quản lý bệnh viện
- Reset mật khẩu mặc định về `123456` cho người bệnh và admin
- Tài khoản bị khóa không được truy cập hệ thống
- Câu hỏi người dùng chỉ được cơ sở y tế trả lời một lần

## Các trang liên quan

- [[huong-dan-su-dung-web-vendor]]
- [[dat-lich-kham]]
- [[xac-thuc-bao-mat]]
- [[zalo-sms]]