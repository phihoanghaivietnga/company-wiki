# Gói dịch vụ

**Tóm tắt**: Module Gói dịch vụ quản lý việc tạo và quản lý các gói dịch vụ khám chữa bệnh, bao gồm khai báo gói, quản lý người bệnh sử dụng gói, miễn giảm, chốt chi phí và thanh toán.

**Nguồn**: GoiDichVu.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Quản lý các gói dịch vụ khám chữa bệnh, cho phép khai báo gói, chỉ định khoa sử dụng, quản lý người bệnh sử dụng gói, thực hiện miễn giảm và chốt chi phí (nguồn: GoiDichVu.pdf).

## Danh mục gói dịch vụ

### Khai báo gói
- Nhập mã gói, tên gói, loại dịch vụ, số ngày sử dụng (nguồn: GoiDichVu.pdf)
- Hệ thống tự động tính hạn sử dụng gói theo số ngày đã khai báo

### Chỉnh sửa thông tin gói
Tìm kiếm và chọn gói cần sửa, chỉnh sửa thông tin và lưu (nguồn: GoiDichVu.pdf).

### Dịch vụ trong gói
- **Thêm mới dịch vụ**: Chọn dịch vụ từ danh mục, thêm vào gói (nguồn: GoiDichVu.pdf)
- **Chỉnh sửa dịch vụ**: Thay đổi dịch vụ trong gói

### Khoa chỉ định gói
- **Thêm khoa chỉ định**: Chọn khoa được phép chỉ định gói (nguồn: GoiDichVu.pdf)
- **Chỉnh sửa khoa chỉ định**: Thay đổi khoa được chỉ định

## Danh sách người bệnh sử dụng gói

### Thêm mới NB sử dụng gói
Có thể gán gói cho NB từ màn hình Tiếp đón hoặc thêm mới tại màn hình Danh sách NB sử dụng gói (nguồn: GoiDichVu.pdf).

### Tìm kiếm, xem chi tiết, xóa gói
- Tìm kiếm theo mã NB, tên NB, tên gói, trạng thái gói, khoa (nguồn: GoiDichVu.pdf)
- Chỉ xóa được gói khi NB chưa sử dụng dịch vụ trong gói

### Tạo miễn giảm gói
Các hình thức miễn giảm (nguồn: GoiDichVu.pdf):
- Miễn giảm theo phiếu thu
- Miễn giảm theo dịch vụ
- Voucher

### Hủy sử dụng gói
Áp dụng khi NB đã thanh toán nhưng quyết định không mua gói nữa (điều kiện: trạng thái gói = Tạo mới). Cho phép sửa số tiền trả lại (nguồn: GoiDichVu.pdf).

### Kết thúc sử dụng gói
Áp dụng khi NB đã sử dụng hết gói (điều kiện: trạng thái gói = Đang sử dụng). Nếu tổng tiền đã thanh toán < tổng tiền gói sau giảm, cần thanh toán thêm (nguồn: GoiDichVu.pdf).

### Chốt chi phí gói
Dành cho nhân viên quản lý, admin (nguồn: GoiDichVu.pdf):
- NB thực hiện nửa liệu trình muốn đổi dịch vụ còn lại
- Chốt chi phí các dịch vụ đã thực hiện, chuyển dịch vụ chưa thực hiện sang gói mới

### Phiếu thu tạm ứng gói dịch vụ
Thanh toán tạm ứng cho gói dịch vụ với nhiều phương thức thanh toán (nguồn: GoiDichVu.pdf).

## Các trang liên quan

- [[tiep-don]]
- [[thu-ngan]]
- [[he-thong-sakura-his]]