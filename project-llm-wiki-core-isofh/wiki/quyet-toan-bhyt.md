# Quyết toán BHYT

**Tóm tắt**: Module Quyết toán Bảo hiểm Y tế quản lý việc tạo hồ sơ XML, gửi giám định lên Cổng BHYT, so sánh dữ liệu giữa HIS và cổng, xuất dữ liệu theo các mẫu quy định (QĐ4210, QĐ130).

**Nguồn**: QuyetToanBHYT.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Liên thông dữ liệu với cổng giám định BHYT, quản lý toàn bộ quy trình quyết toán bảo hiểm y tế cho người bệnh ngoại trú và nội trú (nguồn: QuyetToanBHYT.pdf).

## Danh sách NB chờ tạo hồ sơ quyết toán BHYT

### Điều kiện hiển thị
- **Ngoại trú**: Hồ sơ HIS chưa tạo XML + NB đã được bác sĩ đưa ra hướng điều trị (hẹn khám, cho về, chuyển viện) (nguồn: QuyetToanBHYT.pdf)
- **Nội trú**: Hồ sơ HIS chưa tạo XML + Đã tổng kết ra viện

### Tạo hồ sơ quyết toán hàng loạt
Cho phép tạo hồ sơ XML theo ngày thanh toán, ngày đăng ký/ra viện, đối tượng KCB, năm/tháng quyết toán (nguồn: QuyetToanBHYT.pdf).

## Danh sách hồ sơ giám định BH

Hệ thống hỗ trợ hai mẫu hồ sơ (nguồn: QuyetToanBHYT.pdf):

### Mẫu 79A 46 cột (QĐ4210)
Gồm XML1-5, hỗ trợ các chức năng:
- Xem, sửa, xóa hồ sơ XML
- Gửi giám định (đơn lẻ hoặc hàng loạt)
- Đồng bộ XML chi tiết và đồng bộ dữ liệu
- Tải xuống (tách/gộp XML)
- So sánh dữ liệu trên cổng và HIS (theo các mẫu 19-VTYT, 20-Thuốc, 21-DVKT)
- Xuất dữ liệu XML

### Mẫu QĐ130
Gồm XML0-15, các chức năng tương tự mẫu QĐ4210.

### So sánh dữ liệu cổng và HIS
Tải file XML từ cổng, import vào phần mềm để so sánh, kết quả trả về file excel gồm 4 sheet:
- Hồ sơ có ở HIS - không có trên Cổng
- Hồ sơ có trên Cổng - không có ở HIS
- Hồ sơ khớp nhau
- Hồ sơ lệch tiền

## Danh sách hồ sơ BH đã xóa
Xem lịch sử và lý do các hồ sơ đã tạo và bị xóa (nguồn: QuyetToanBHYT.pdf).

## Các trang liên quan

- [[thu-ngan]]
- [[ke-hoach-tong-hop]]
- [[quy-trinh-kham-ngoai-tru]]
- [[quan-ly-noi-tru]]