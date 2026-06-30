# Hồ sơ sức khỏe điện tử

**Tóm tắt**: Chức năng cho phép người bệnh tra cứu và xem hồ sơ sức khỏe, kết quả khám chữa bệnh ngoại trú trên Web và App.

**Nguồn**: 04_plhd_app_03_03.md, 05_web_tra_cuu_hssk_bv_mat_vn.md, 11_hdsd_web_tra_cuu.md

**Cập nhật lần cuối**: 13/06/2026

---

## Phạm vi hỗ trợ

- Hỗ trợ xem trên website (nguồn: 04_plhd_app_03_03.md)
- Hỗ trợ xem trên app

## Các loại kết quả khám ngoại trú

Người bệnh có thể xem các thông tin sau:
- Khám thông thường (nguồn: 04_plhd_app_03_03.md)
- Kết quả xét nghiệm
- Kết quả chẩn đoán hình ảnh (CĐHA)
- Xem hình ảnh PACS
- Danh sách dịch vụ đã sử dụng
- Kết quả khám
- Kết quả CĐHA và thăm dò chức năng
- Phiếu thuật thủ thuật
- Thuốc

## Các bước tra cứu kết quả khám

1. Tại trang chủ, chọn ô chức năng "Hồ sơ sức khỏe" (nguồn: 11_hdsd_web_tra_cuu.md)
2. Nhấn nút "Tra cứu kết quả khám bệnh"
3. Điền thông tin theo yêu cầu tại popup
4. Nếu tài khoản có nhiều hồ sơ, chọn đúng hồ sơ cần tra cứu
5. Nhập mã bệnh nhân/mã hồ sơ và nhấn "Tra cứu"
6. Nhấn "Xem kết quả khám bệnh" để mở danh sách kết quả

## Xem hồ sơ sức khỏe

- Tại màn hình Hồ sơ sức khỏe, chọn hồ sơ người bệnh ở danh sách bên trái (nguồn: 11_hdsd_web_tra_cuu.md)
- Nhấn vào thẻ kết quả cần xem chi tiết
- Kiểm tra thông tin ngày khám và các nhóm kết quả:
  - **Kết quả khám bệnh**: Kết luận chẩn đoán, chỉ định
  - **Cận lâm sàng**: Kết quả xét nghiệm, CĐHA
  - **Thuốc**: Đơn thuốc, hướng dẫn sử dụng

## Thông tin định danh trên hồ sơ

- Họ tên, giới tính/tuổi (nguồn: 04_plhd_app_03_03.md)
- Mã người bệnh, mã hồ sơ
- QR code

## Yêu cầu bảo mật

- Hồ sơ sức khỏe là dữ liệu nhạy cảm, cần xác thực chặt (nguồn: 05_web_tra_cuu_hssk_bv_mat_vn.md)
- Cần xác định cơ chế đăng nhập bằng mật khẩu, OTP hoặc cả hai
- Cần xác định timeout phiên người dùng
- Cần ghi log truy cập hồ sơ sức khỏe
- Cần xác định chính sách che/masking dữ liệu cá nhân nếu hiển thị công khai trên màn hình

## Các vấn đề cần làm rõ

- Người bệnh xác thực bằng gì để xem hồ sơ sức khỏe? (nguồn: 10_open_questions.md)
- Có yêu cầu OTP mỗi lần xem kết quả không?
- PACS được xem qua link, iframe, viewer tích hợp hay tải ảnh?
- Có giới hạn thời gian lưu hoặc thời gian truy cập kết quả không?
- Dữ liệu nào lấy từ HIS, dữ liệu nào lưu tại hệ thống ISOFH?

## Các trang liên quan

- [[dat-lich-kham]]
- [[dong-bo-du-lieu-his]]
- [[xac-thuc-bao-mat]]
- [[huong-dan-su-dung-web-tra-cuu]]