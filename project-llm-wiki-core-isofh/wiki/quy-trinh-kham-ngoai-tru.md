# Quy trình khám ngoại trú

**Tóm tắt**: Mô tả luồng quy trình khám chữa bệnh ngoại trú tại bệnh viện sử dụng hệ thống Sakura HIS, từ khi người bệnh đến lấy số cho đến khi kết thúc và ra về.

**Nguồn**: 20260522_luong_khach_hang_den_kham.md, TiepDon.pdf, KhamBenh.pdf, ThuNgan.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Luồng cơ bản

Quy trình khám ngoại trú cơ bản diễn ra theo các bước sau (nguồn: 20260522_luong_khach_hang_den_kham.md):

1. **Lấy số & Tiếp đón**: Khách hàng đến lấy số, Lễ tân chỉ định dịch vụ khám dựa trên yêu cầu (Module [[tiep-don]])
2. **Thu ngân**: Thu tiền & in phiếu (Module [[thu-ngan]])
3. **Quay lại Lễ tân**: Nhận chỉ định và giấy tờ (Module [[tiep-don]])
4. **Thực hiện CĐHA/TDCN**: Đo thị lực, Chẩn đoán hình ảnh để lấy kết quả (Module [[chan-doan-hinh-anh-tdcn]])
5. **Vào phòng khám**: Khám bệnh, chẩn đoán (Module [[kham-benh]])

## Hướng điều trị sau khám

Sau khi khám, bác sĩ có các hướng điều trị sau (nguồn: KhamBenh.pdf, 20260522_luong_khach_hang_den_kham.md):

- **Cho về**: Bác sĩ kết luận và chỉ định thuốc → Người bệnh ra nhà thuốc lấy thuốc (Module [[nha-thuoc]]) → ra về
- **Hẹn khám**: Hẹn tái khám vào ngày cụ thể
- **Chuyển viện**: Chuyển tuyến đến cơ sở y tế khác
- **Nhập viện**: Chỉ định nội trú (Module [[quan-ly-noi-tru]]) → Xét nghiệm (Module [[xet-nghiem]]) → Kết luận xét nghiệm → Phẫu thuật nếu đủ điều kiện (Module [[phau-thuat-thu-thuat]]) → Chỉ định thuốc (Module [[nha-thuoc]]) → ra về
- **Không khám**: Người bệnh bỏ về, cần thao tác đóng hồ sơ

## Quy trình thanh toán

Người bệnh có thể thanh toán theo các hình thức:
- **Thanh toán trực tiếp** tại quầy thu ngân (nguồn: ThuNgan.pdf)
- **Thanh toán không tiền mặt** qua QR code (nguồn: ThuNgan.pdf)
- **Tạm ứng** trước khi thực hiện dịch vụ (nguồn: TiepDon.pdf)

## Quy trình BHYT

- Người bệnh khám BHYT được thực hiện dịch vụ BHYT chi trả trước, thanh toán sau (nguồn: KhamBenh.pdf)
- Nếu có phát sinh dịch vụ BHYT không chi trả thì phải tạm ứng hoặc thanh toán dịch vụ đó
- Sau khi kết thúc khám, cần duyệt bảo hiểm trước khi thanh toán và phát thuốc (nguồn: KeHoachTongHop.pdf)

## Các trang liên quan

- [[tiep-don]]
- [[kham-benh]]
- [[thu-ngan]]
- [[chan-doan-hinh-anh-tdcn]]
- [[xet-nghiem]]
- [[nha-thuoc]]
- [[quan-ly-noi-tru]]
- [[phau-thuat-thu-thuat]]
- [[ke-hoach-tong-hop]]
- [[quyet-toan-bhyt]]