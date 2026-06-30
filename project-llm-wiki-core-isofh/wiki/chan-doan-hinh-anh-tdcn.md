# Chẩn đoán hình ảnh & Thăm dò chức năng

**Tóm tắt**: Module quản lý quy trình thực hiện các dịch vụ chẩn đoán hình ảnh (CĐHA) và thăm dò chức năng (TDCN), từ tiếp nhận người bệnh, phân phòng, thực hiện đến trả kết quả.

**Nguồn**: CDHA_TDCN.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Tiếp nhận người bệnh thực hiện dịch vụ Chẩn đoán hình ảnh & Thăm dò chức năng, ghi nhận thông tin, trả kết quả thực hiện của NB và lưu vào hệ thống (nguồn: CDHA_TDCN.pdf).

## Luồng quy trình

Chẩn đoán hình ảnh & Thăm dò chức năng → Tiếp nhận NB thực hiện dịch vụ và phân phòng thực hiện, sinh STT thực hiện → Gọi NB vào phòng theo STT → Trả kết quả (nguồn: CDHA_TDCN.pdf).

## Các bước thực hiện

### Chờ tiếp đón
Vào màn hình CĐHA-TDCN, chọn chờ tiếp đón để xem danh sách NB chờ (nguồn: CDHA_TDCN.pdf).

### Phân phòng
Chọn dịch vụ, phân phòng thực hiện cho NB (nguồn: CDHA_TDCN.pdf).

### Tiếp nhận
- Chọn phòng thực hiện, bác sĩ, điều dưỡng (nguồn: CDHA_TDCN.pdf)
- Tích chọn dịch vụ và tiếp nhận

### Tiếp nhận nhiều NB KSK
Hỗ trợ tiếp đón nhiều NB khám sức khỏe cùng một lúc, tìm kiếm theo hợp đồng, công ty, phòng ban/lớp (nguồn: CDHA_TDCN.pdf).

### Trả kết quả
- Lọc theo trạng thái "Đã tiếp nhận" (nguồn: CDHA_TDCN.pdf)
- Nhập kết quả, kết luận, cách thức/phương thức can thiệp
- Hỗ trợ chọn mẫu kết quả tự động điền
- Tích hợp PACS: Kết quả được đồng bộ từ PACS về HIS
- In kết quả sau khi hoàn thành

## Trạng thái dịch vụ

Các trạng thái cơ bản: Chờ tiếp nhận, Đã tiếp nhận, Đã có kết quả (nguồn: CDHA_TDCN.pdf).

- Đổi từ Đã có kết quả về Đã tiếp nhận: Chọn "Hủy kết quả"
- Đổi từ Đã tiếp nhận về Chờ tiếp nhận: Chọn "Hủy tiếp nhận"

## Xử lý tình huống

- **Không tìm thấy NB**: Chọn đúng phòng thực hiện hoặc chọn tất cả phòng; kiểm tra lại bộ lọc trạng thái (nguồn: CDHA_TDCN.pdf)
- **Yêu cầu hoàn dịch vụ**: Hoàn ở màn hình danh sách hoặc chi tiết; dịch vụ chuyển trạng thái "Chờ duyệt hoàn" (nguồn: CDHA_TDCN.pdf)
- **Yêu cầu đổi dịch vụ**: Chọn dịch vụ mới, phòng thực hiện, lý do đổi (nguồn: CDHA_TDCN.pdf)
- **Xem file kết quả từ PACS**: Dịch vụ ở trạng thái "Đã có kết quả" mới có thể xem file PDF/ảnh (nguồn: CDHA_TDCN.pdf)

## Các trang liên quan

- [[kham-benh]]
- [[quy-trinh-kham-ngoai-tru]]
- [[xet-nghiem]]