# Tiếp đón

**Tóm tắt**: Module Tiếp đón là bước đầu tiên trong quy trình khám chữa bệnh, thực hiện tiếp nhận thông tin người bệnh, chỉ định dịch vụ khám, thu tạm ứng và in phiếu khám.

**Nguồn**: TiepDon.pdf, 20260522_luong_khach_hang_den_kham.md

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Tiếp đón, ghi nhận thông tin người bệnh (NB) và lưu vào hệ thống, chỉ định các dịch vụ để người bệnh thực hiện quá trình khám chữa bệnh (nguồn: TiepDon.pdf).

## Luồng quy trình

Tiếp đón → Nhập thông tin người bệnh → Kê dịch vụ → Tạm ứng → In phiếu khám (nguồn: TiepDon.pdf).

Luồng tiếp đón được thực hiện chủ yếu trên 2 màn hình: tiếp đón và kê dịch vụ.

## Chức năng chính

### Nhập thông tin người bệnh
- **Thông tin chung**: Quét QR CCCD, BHYT, nhập lý do đến khám, chọn đối tượng KCB, phân loại NB (ưu tiên, cấp cứu, KSK) (nguồn: TiepDon.pdf)
- **Thông tin cá nhân**: Họ tên, giới tính, địa chỉ (hỗ trợ nhập viết tắt), số giấy tờ tùy thân (nguồn: TiepDon.pdf)
- **Thông tin BHYT**: Kiểm tra thẻ qua cổng giám định, hỗ trợ thẻ hộ nghèo/cận nghèo (nguồn: TiepDon.pdf)
- **Thông tin bổ sung**: Người bảo lãnh (bắt buộc với NB < 6 tuổi), nguồn NB, người giới thiệu (nguồn: TiepDon.pdf)
- **Sinh hiệu**: Nhập các chỉ số đo của NB (nguồn: TiepDon.pdf)
- **Bắt trùng thông tin**: Kiểm tra trùng NB dựa trên 5 tiêu chí (tên+địa chỉ, SĐT, SĐT người bảo lãnh, CMND/CCCD, số thẻ BHYT) (nguồn: TiepDon.pdf)

### Chỉ định dịch vụ
- Tìm kiếm và kê dịch vụ khám cho người bệnh (nguồn: TiepDon.pdf)
- Thu tạm ứng trước khi thực hiện dịch vụ (nguồn: TiepDon.pdf)
- In phiếu khám cho người bệnh (nguồn: TiepDon.pdf)

### Chức năng khác
- **Danh sách NB đã tiếp đón**: Tra cứu, lọc, sắp xếp, đổi dịch vụ khám (nguồn: TiepDon.pdf)
- **Danh sách NB hủy tiếp đón**: Quản lý NB đã hủy (nguồn: TiepDon.pdf)
- **Danh sách lịch hẹn**: Quản lý lịch hẹn tái khám, nhắc lịch (nguồn: TiepDon.pdf)
- **Tiếp nhận nhiều NB KSK**: Tiếp đón hàng loạt NB khám sức khỏe (nguồn: TiepDon.pdf)
- **Tạo thẻ tạm**: Cho NB dưới 6 tuổi chưa có thẻ BHYT (nguồn: TiepDon.pdf)

### Xử lý tình huống
- Mất kết nối cổng giám định BHYT: Báo CNTT, có thể bỏ qua thẻ nhưng dịch vụ sẽ không được BHYT quyết toán (nguồn: TiepDon.pdf)

## Các trang liên quan

- [[quy-trinh-kham-ngoai-tru]]
- [[kham-benh]]
- [[thu-ngan]]
- [[chan-doan-hinh-anh-tdcn]]
- [[xet-nghiem]]