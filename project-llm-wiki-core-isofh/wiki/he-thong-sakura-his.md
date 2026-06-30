# Hệ thống Sakura HIS

**Tóm tắt**: Tổng quan về Giải pháp Quản lý Bệnh viện SAKURA - một hệ thống thông tin bệnh viện (HIS) tích hợp toàn diện, bao gồm 16 phân hệ chức năng phục vụ quy trình khám chữa bệnh từ tiếp đón đến thanh toán và quyết toán bảo hiểm.

**Nguồn**: 20260522_luong_khach_hang_den_kham.md, TiepDon.pdf, KhamBenh.pdf, CDHA_TDCN.pdf, XetNghiem.pdf, ThuNgan.pdf, NhaThuoc.pdf, QuanLyNoiTru.pdf, PhauThuatThuThuat.pdf, HoSoBenhAn.pdf, KeHoachTongHop.pdf, GoiDichVu.pdf, QuanLyKho.pdf, KySo.pdf, QuyetToanBHYT.pdf, Thuoc.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Giới thiệu

Sakura HIS là giải pháp quản lý bệnh viện toàn diện, phiên bản V1.6 (các phân hệ Kho và Nội trú ở phiên bản V1.4). Hệ thống bao phủ toàn bộ quy trình nghiệp vụ của một bệnh viện, từ khâu tiếp đón người bệnh, khám bệnh, chỉ định dịch vụ kỹ thuật, xét nghiệm, chẩn đoán hình ảnh, phẫu thuật, quản lý nội trú, nhà thuốc, thanh toán, đến quyết toán bảo hiểm y tế (nguồn: TiepDon.pdf, KhamBenh.pdf, XetNghiem.pdf, NhaThuoc.pdf).

## Kiến trúc phân hệ

Hệ thống được chia thành các nhóm phân hệ chính:

### Nhóm quy trình khám ngoại trú
- [[tiep-don]]: Tiếp đón và nhập thông tin người bệnh
- [[kham-benh]]: Khám bệnh, chỉ định dịch vụ kỹ thuật, kê đơn
- [[chan-doan-hinh-anh-tdcn]]: Chẩn đoán hình ảnh & Thăm dò chức năng
- [[xet-nghiem]]: Xét nghiệm và trả kết quả
- [[nha-thuoc]]: Nhà thuốc - phát thuốc
- [[thu-ngan]]: Thu ngân - thanh toán, xuất hóa đơn điện tử

### Nhóm quản lý nội trú
- [[quan-ly-noi-tru]]: Điều trị nội trú, tờ điều trị, kê phòng giường
- [[phau-thuat-thu-thuat]]: Quản lý phẫu thuật và thủ thuật

### Nhóm hành chính - hồ sơ
- [[ho-so-benh-an]]: Quản lý hồ sơ bệnh án điện tử
- [[ke-hoach-tong-hop]]: Lưu trữ bệnh án, duyệt bảo hiểm

### Nhóm quản lý kinh doanh - kho
- [[goi-dich-vu]]: Quản lý gói dịch vụ
- [[quan-ly-kho]]: Quản trị kho, nhập/xuất, dược lâm sàng

### Nhóm bảo hiểm - kế toán
- [[quyet-toan-bhyt]]: Quyết toán bảo hiểm y tế, liên thông cổng giám định

### Nhóm tích hợp khác
- [[ky-so]]: Ký số, ký điện tử

## Tích hợp hệ thống

Sakura HIS hỗ trợ tích hợp với nhiều hệ thống chuyên ngành:
- **LIS** (Laboratory Information System): Nhận kết quả xét nghiệm tự động (nguồn: XetNghiem.pdf)
- **PACS** (Picture Archiving and Communication System): Nhận kết quả hình ảnh CĐHA (nguồn: CDHA_TDCN.pdf)
- **Cổng giám định BHYT**: Liên thông dữ liệu XML lên cổng BHYT (nguồn: QuyetToanBHYT.pdf)
- **Hệ thống GPP**: Liên thông nhà thuốc (nguồn: NhaThuoc.pdf)
- **Phần mềm tài chính kế toán**: Đồng bộ chứng từ tự động (nguồn: ThuNgan.pdf)

## Các trang liên quan

- [[quy-trinh-kham-ngoai-tru]]
- [[tiep-don]]
- [[kham-benh]]
- [[thu-ngan]]
- [[nha-thuoc]]
- [[quan-ly-noi-tru]]
- [[quyet-toan-bhyt]]