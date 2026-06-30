# Phụ lục hợp đồng / danh sách phân hệ tính năng App 03.03

## Metadata

- Source file: `4_PLHĐ app 03.03.docx`
- Loại tài liệu: Phụ lục/danh sách phân hệ tính năng
- Chủ đề: Tính năng Mobile App/Web liên quan đặt khám, hồ sơ sức khỏe, thông báo, SMS, thanh toán

## 1. Nhóm tính năng đặt khám

### Đặt khám dịch vụ

- Người dùng có thể đặt khám dịch vụ.
- Giao diện có luồng chọn chuyên khoa/bác sĩ/ngày giờ.

### Đặt lịch khám theo ngày, giờ

- Hỗ trợ chọn ngày khám.
- Hỗ trợ chọn khung giờ khám.
- Có phân chia buổi sáng/chiều theo khung giờ.
- Người dùng nhấn tiếp tục sau khi chọn khung giờ.

### Ràng buộc slot khám

- Không cho phép đặt lịch vào các khung giờ đã có bệnh nhân đặt lịch đủ.
- Hệ thống cần có cơ chế kiểm tra số lượng đặt khám theo slot.

### Đồng bộ HIS

- Lịch khám phải được đồng bộ sang hệ thống HIS.
- Đây là yêu cầu tích hợp quan trọng, cần đặc tả API/luồng đồng bộ riêng.

## 2. Hồ sơ sức khỏe điện tử

### Phạm vi hỗ trợ

- Hỗ trợ xem trên website.
- Hỗ trợ xem trên app.

### Xem kết quả khám chữa bệnh cho người bệnh ngoại trú

Các loại thông tin/kết quả được nhắc tới:

- Khám thông thường.
- Kết quả xét nghiệm.
- Kết quả chẩn đoán hình ảnh.
- Xem hình ảnh PACS.
- Danh sách dịch vụ.
- Kết quả khám.
- Kết quả CĐHA và thăm dò chức năng.
- Phiếu thuật thủ thuật.
- Thuốc.

### Thông tin định danh trên hồ sơ

Ảnh minh họa có thông tin người bệnh như:

- Họ tên.
- Giới tính/tuổi.
- Mã người bệnh.
- Mã hồ sơ.
- QR code.

## 3. Quản lý hồ sơ & kết quả đặt khám cho người thân

- Hỗ trợ quản lý hồ sơ người thân.
- Hỗ trợ xem kết quả hoặc hồ sơ liên quan người thân.
- Hỗ trợ đặt khám cho người thân.

## 4. Thông báo & tin tức

### Thông báo

- Người dùng nhận thông báo.
- Người dùng xem thông báo trên app.
- Có ghi chú cần hướng dẫn CNTT setup tính năng này.

### Tin tức

- Người dùng xem tin tức/bài viết trên app.
- Màn hình app có khu vực tin tức, danh sách bài viết và điều hướng xem thêm.

## 5. Gửi Zalo SMS qua Zalo bệnh viện

Các tình huống gửi Zalo SMS được nhắc tới:

- Gửi OTP khi người bệnh đăng ký tài khoản.
- Gửi OTP khi người bệnh quên mật khẩu.
- Gửi thông tin tài khoản người bệnh khám sức khỏe.
- Gửi tin nhắn đặt khám thành công.

## 6. Thanh toán trực tuyến

- Hiện tại hệ thống chưa hỗ trợ thanh toán trực tuyến qua web/app tra cứu hồ sơ khám bệnh

## 7. Các điểm cần đặc tả thêm

- Danh sách chính xác các trạng thái lịch khám.
- Quy tắc giới hạn số lượng bệnh nhân theo slot.
- Cơ chế đồng bộ hai chiều hay một chiều với HIS.
- API lấy kết quả khám, xét nghiệm, CĐHA, PACS.
- Quy định bảo mật khi hiển thị hồ sơ sức khỏe và QR.
- Quy tắc người thân: ai được xem, xác thực quan hệ thế nào.
- Kịch bản thanh toán thành công/thất bại/hết hạn QR/hoàn tiền.