# Web tra cứu HSSK - BV Mắt Việt Nga

## Metadata

- Source file: `5_Web tra cứu HSSK BV Mắt VN.docx`
- Loại tài liệu: Tài liệu mô tả Web tra cứu hồ sơ sức khỏe
- Chủ đề: Website tra cứu HSSK/kết quả khám cho Bệnh viện Mắt Việt Nga

## Mục tiêu hệ thống

Cung cấp website cho phép người bệnh tra cứu hồ sơ sức khỏe/kết quả khám chữa bệnh. Hệ thống có liên quan đến dữ liệu HIS và các kết quả khám chữa bệnh của người bệnh.

## Nhóm chức năng dự kiến

- Người bệnh truy cập website tra cứu.
- Xác thực người bệnh trước khi xem dữ liệu y tế.
- Tra cứu hồ sơ sức khỏe.
- Xem kết quả khám chữa bệnh.
- Xem danh sách dịch vụ đã sử dụng.
- Xem kết quả xét nghiệm/chẩn đoán hình ảnh nếu có.
- Có thể liên quan tới QR code hoặc thông tin định danh người bệnh.

## Yêu cầu bảo mật cần lưu ý

- Hồ sơ sức khỏe là dữ liệu nhạy cảm, cần xác thực chặt.
- Cần xác định cơ chế đăng nhập bằng mật khẩu, OTP hoặc cả hai.
- Cần xác định timeout phiên người dùng.
- Cần ghi log truy cập hồ sơ sức khỏe.
- Cần xác định chính sách che/masking dữ liệu cá nhân nếu hiển thị công khai trên màn hình.

## Liên hệ tài liệu khác

- Liên quan trực tiếp tới `04_plhd_app_03_03.md`, mục Hồ sơ sức khỏe điện tử.
- Liên quan tới `03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md`, mục kiến trúc, xác thực, bảo mật, SSL/TLS.
- Có thể liên quan tới `08_so_do_he_thong_webapp_20260508.md` để xác định luồng kết nối.

## Trạng thái tổng hợp hiện tại

Chưa có đủ đoạn nội dung chi tiết trong ngữ cảnh làm việc để chuyển toàn văn tài liệu này thành Markdown hoàn chỉnh. Cần ingest lại file DOCX gốc để lấy đầy đủ màn hình, luồng tra cứu và quy tắc xác thực.