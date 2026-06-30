# Nhà thuốc

**Tóm tắt**: Module Nhà thuốc quản lý quy trình phát thuốc cho người bệnh, từ tư vấn đơn thuốc, kê thêm thuốc, thanh toán đến phát thuốc, hỗ trợ liên thông GPP và xuất hóa đơn điện tử.

**Nguồn**: NhaThuoc.pdf, Thuoc.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Quản lý việc phát thuốc cho người bệnh theo đơn của bác sĩ, bao gồm tư vấn đơn thuốc (dược lâm sàng), kê thêm/thay đổi thuốc, thanh toán và phát thuốc (nguồn: NhaThuoc.pdf).

## Quy trình thanh toán - phát thuốc

1. NB ra quầy Nhà thuốc quét đơn thuốc (nguồn: NhaThuoc.pdf)
2. Bộ phận Dược lâm sàng kiểm kê, tư vấn thêm thuốc (nếu có)
3. NB trở lại quầy Thu ngân thanh toán tiền đơn thuốc
4. NB cầm hóa đơn thanh toán kèm đơn thuốc quay lại Nhà thuốc để nhận thuốc

## Các chức năng chính

### Tìm kiếm người bệnh
- Quét QR người bệnh, nhập mã hồ sơ hoặc tên NB (nguồn: NhaThuoc.pdf)
- Lọc theo quầy, kho thuốc, trạng thái đơn (Tạo mới/Đã phát)

### Tư vấn đơn thuốc
Cho phép nhân viên nhà thuốc thay đổi so với đơn gốc của bác sĩ (nguồn: NhaThuoc.pdf):
- **Kê thêm thuốc mới**: Nhập tên thuốc, số lượng, liều dùng, ghi chú
- **Thay đổi số lượng bán**: Nhập số lượng mới
- **Xóa thuốc**: Xóa thuốc khỏi đơn
- **Sửa số lô**: Chọn số lô khác cho thuốc

### Tạo miễn giảm
Hỗ trợ miễn giảm theo dịch vụ hoặc voucher (nguồn: NhaThuoc.pdf).

### Danh sách hóa đơn điện tử
- Xuất hóa đơn cho 1 người hoặc nhiều người (nguồn: NhaThuoc.pdf)
- Hỗ trợ xuất hóa đơn cho công ty

### Sửa phương thức thanh toán
Cho phép sửa, thêm, xóa phương thức thanh toán tại màn hình Nhà thuốc (nguồn: NhaThuoc.pdf).

### Tạo đơn thuốc cho khách vãng lai
Cho phép tạo đơn thuốc cho người bệnh không có mã NB trong hệ thống (nguồn: NhaThuoc.pdf).

## Liên thông GPP

Hỗ trợ đẩy dữ liệu liên thông lên hệ thống quản lý nhà thuốc đạt chuẩn GPP (nguồn: NhaThuoc.pdf):
- Loại đẩy: hóa đơn bán thuốc, phiếu nhập, phiếu xuất
- Hỗ trợ đẩy đơn lẻ hoặc hàng loạt

## Kiểm tra tồn thuốc, hạn sử dụng

Tra cứu tồn kho, hạn sử dụng của thuốc, xem danh sách nhập/xuất (nguồn: NhaThuoc.pdf).

## Các báo cáo nhà thuốc

Hệ thống cung cấp các báo cáo: KNT01-KNT15, bao gồm báo cáo thống kê thuốc bán theo bác sĩ, tổng hợp thu tiền quầy, chi tiết thu theo ngày, báo cáo ADR (phản ứng có hại của thuốc), báo cáo xuất nhập tồn, v.v. (nguồn: NhaThuoc.pdf).

## Các trang liên quan

- [[kham-benh]]
- [[thu-ngan]]
- [[quy-trinh-kham-ngoai-tru]]
- [[quan-ly-kho]]