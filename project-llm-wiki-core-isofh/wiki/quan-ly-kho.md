# Quản lý kho

**Tóm tắt**: Module Quản lý kho (Kho dược) quản lý toàn bộ hoạt động kho của bệnh viện, bao gồm quản trị kho, quản lý thầu (đấu thầu thuốc/vật tư), nhập kho, xuất kho, tồn kho, phát thuốc ngoại trú/ra viện và duyệt dược lâm sàng.

**Nguồn**: QuanLyKho.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Quản trị kho, thiết lập kho chỉ định, quản lý thầu, quản lý nhập/xuất hàng hóa, cấp phát thuốc ngoại trú (nguồn: QuanLyKho.pdf).

## Quản trị kho

### Thêm mới kho
Khai báo thông tin kho mới: mã kho, tên kho, loại kho (nguồn: QuanLyKho.pdf).

### Kho trực thuộc
Thiết lập quan hệ kho cha - kho con, cho phép tạo phiếu dự trù, phiếu lĩnh bù tủ trực, phiếu xuất từ kho con lên kho cha (nguồn: QuanLyKho.pdf).

### Thiết lập kho chỉ định
Cấu hình kho được phép chỉ định cho từng loại đối tượng NB (nguồn: QuanLyKho.pdf).

### Nhân viên quản lý
Gán nhân viên quản lý cho từng kho (nguồn: QuanLyKho.pdf).

## Quản lý thầu

Quản lý thông tin thầu và hàng hóa thuộc thầu (nguồn: QuanLyKho.pdf):
- Quyết định thầu: Thêm mới, hoàn thành, duyệt, hủy duyệt
- Chi tiết thầu: Thêm dịch vụ trong thầu, quản lý giá, tỷ lệ thanh toán BH
- Khai báo giảm giá: Giảm giá thuốc/VTYT/Hóa chất theo thầu
- Khai báo cơ số thầu: Tăng/giảm cơ số, vượt 20%, điều tiết, tùy chọn mua thêm

## Nhập kho

Các hình thức nhập (nguồn: QuanLyKho.pdf):
- **Phiếu nhập dự trù**: Nhập hàng từ kho xuất
- **Phiếu nhập từ nhà cung cấp**: Nhập hàng hóa mới (theo thầu hoặc không theo thầu)
- **Phiếu lĩnh bù**: Bổ sung tủ trực
- **Phiếu nhập khác**: Tặng, tài trợ

Quy trình: Tạo phiếu → Gửi duyệt → Duyệt → Hoàn thành.

## Xuất kho

Các hình thức xuất: xuất trả kho-kho, xuất hủy, xuất trả nhà cung cấp (nguồn: QuanLyKho.pdf). Hỗ trợ chọn lô cụ thể theo FIFO hoặc tùy chọn.

## Danh sách tồn kho

Tra cứu tồn kho theo hàng hóa, lô, hạn sử dụng. Hỗ trợ thiết lập (nguồn: QuanLyKho.pdf):
- **Khóa DV**: Khóa thuốc theo đối tượng (ngoại trú, nội trú...)
- **Khoa được truy cập**: Giới hạn khoa được chỉ định
- **Đối tượng được phép kê**: Giới hạn đối tượng NB
- **Tài khoản được truy cập**: Giới hạn tài khoản được chỉ định

## Phát thuốc ngoại trú, ra viện

Phát thuốc cho NB ngoại trú từ kho, hỗ trợ hủy phát (nguồn: QuanLyKho.pdf). NB cần thanh toán tại Thu ngân trước khi nhận thuốc.

## Duyệt dược lâm sàng

Quy trình duyệt đơn thuốc trước khi phát (nguồn: QuanLyKho.pdf):
- **Duyệt DLS**: Duyệt đơn thuốc từ bác sĩ
- **Can thiệp dược**: Từ chối duyệt và ghi chú cho bác sĩ
- **Xác nhận DLS**: Bác sĩ phản hồi về can thiệp dược
- **Kiểm tra tương tác thuốc**: Cảnh báo tương tác thuốc
- **Tư vấn thuốc**: Tư vấn cho NB về cách dùng, tác dụng phụ

## Các báo cáo kho

Hệ thống cung cấp nhiều báo cáo K01-K38, bao gồm: bảng kê nhập/xuất, tồn kho, thẻ kho, biên bản kiểm kê, báo cáo thuốc sử dụng theo khoa, báo cáo kháng sinh, danh sách NB tái nhập viện, báo cáo tương tác thuốc (nguồn: QuanLyKho.pdf).

## Các trang liên quan

- [[nha-thuoc]]
- [[quan-ly-noi-tru]]
- [[phau-thuat-thu-thuat]]
- [[thu-ngan]]
- [[he-thong-sakura-his]]