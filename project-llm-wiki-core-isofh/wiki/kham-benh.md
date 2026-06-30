# Khám bệnh

**Tóm tắt**: Module Khám bệnh là phân hệ trung tâm cho phép bác sĩ thực hiện khám lâm sàng, chỉ định dịch vụ kỹ thuật, xem kết quả, kê đơn thuốc và đưa ra hướng điều trị cho người bệnh.

**Nguồn**: KhamBenh.pdf, 20260522_luong_khach_hang_den_kham.md

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Khám bệnh, chỉ định dịch vụ kỹ thuật (DVKT), vật tư, thuốc, ghi nhận thông tin khám cơ bản của NB để đưa ra hướng điều trị và lưu vào hệ thống (nguồn: KhamBenh.pdf).

## Luồng quy trình

Khám bệnh → Gọi khám → Hỏi bệnh, điền thông tin chẩn đoán và sinh hiệu → Chỉ định DVKT → In phiếu chỉ định (nguồn: KhamBenh.pdf).

## Các bước khám lâm sàng

1. **Nhập chỉ số sinh hiệu** (nguồn: KhamBenh.pdf)
2. **Hỏi bệnh**: Nhập quá trình bệnh lý, tiền sử bệnh, dị ứng thuốc
3. **Khám xét**: Nhập thông tin khám
4. **Chẩn đoán**: Tra cứu theo mã ICD, chẩn đoán chính, chẩn đoán kèm theo, chẩn đoán sơ bộ
5. **Lưu thông tin**: Nhấn Lưu để ghi nhận dữ liệu khám

## Chỉ định dịch vụ kỹ thuật

- Chọn loại dịch vụ (Khám, Xét nghiệm, CĐHA-TDCN) (nguồn: KhamBenh.pdf)
- Tìm kiếm và chọn dịch vụ, tích chọn nhiều dịch vụ cùng lúc
- Có thể sử dụng **bộ chỉ định** (preset) để kê nhanh các dịch vụ
- Tick **Cấp cứu** để đánh dấu dịch vụ ưu tiên
- In phiếu chỉ định sau khi kê

## Xem kết quả dịch vụ

Sau khi NB thực hiện các DVKT cận lâm sàng, bác sĩ có thể xem kết quả tại tab kết quả CĐHA-TDCN (nguồn: KhamBenh.pdf).

## Kết thúc khám

Sau khi có kết quả, bác sĩ đưa ra hướng điều trị (nguồn: KhamBenh.pdf, 20260522_luong_khach_hang_den_kham.md):
- **Cho về**: Không cần điều trị thêm
- **Hẹn khám**: Nhập ngày hẹn tái khám
- **Chuyển viện**: Nhập lý do, phương tiện vận chuyển
- **Nhập viện**: Lập bệnh án nội trú, chọn khoa điều trị
- **Không khám**: Đóng hồ sơ
- Kết quả điều trị: Khỏi, Đỡ, Không thay đổi, Tử vong, Tử vong ngoại viện, Không đánh giá

## Kê đơn thuốc

- Chọn nhà thuốc, kho thuốc (nguồn: KhamBenh.pdf)
- Nhập tên thuốc, số lượng, liều dùng (sáng/trưa/chiều/tối)
- Hỗ trợ chọn nhiều thuốc cùng lúc
- Tự động tính ngày hẹn khám dựa trên số ngày đơn
- In đơn thuốc

## Xử lý tình huống

- **Không thấy NB**: Chọn lại bộ lọc "Ngày đăng ký" (nguồn: KhamBenh.pdf)
- **Phân biệt BHYT**: Dịch vụ BHYT chi trả có đơn giá bảo hiểm khác 0; dịch vụ không được BHYT chi trả có tên kết thúc là "Dịch vụ"
- **Kết thúc khám thất bại**: Do dịch vụ chưa hoàn thành (chưa có kết quả)
- **Sửa/Xóa dịch vụ**: Chỉ xóa được khi chưa thanh toán, chưa thực hiện

## Các trang liên quan

- [[tiep-don]]
- [[quy-trinh-kham-ngoai-tru]]
- [[chan-doan-hinh-anh-tdcn]]
- [[xet-nghiem]]
- [[nha-thuoc]]
- [[quan-ly-noi-tru]]