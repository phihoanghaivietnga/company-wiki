# Xét nghiệm

**Tóm tắt**: Module Xét nghiệm quản lý toàn bộ quy trình xét nghiệm từ lấy mẫu bệnh phẩm, thực hiện xét nghiệm (sinh hóa, huyết học, giải phẫu bệnh, vi sinh) đến trả kết quả, hỗ trợ tích hợp với hệ thống LIS.

**Nguồn**: XetNghiem.pdf

**Cập nhật lần cuối**: 2026-05-22

---

## Mục đích

Quản lý quy trình xét nghiệm cho người bệnh, từ khâu lấy mẫu bệnh phẩm, thực hiện các loại xét nghiệm (sinh hóa, huyết học, giải phẫu bệnh, vi sinh) đến trả kết quả và lưu vào hệ thống (nguồn: XetNghiem.pdf).

## Lấy mẫu bệnh phẩm

### Điều kiện
Người bệnh có phiếu chỉ định của bác sĩ đi xét nghiệm (nguồn: XetNghiem.pdf).

### Các bước
1. Chọn phòng thực hiện
2. Tìm kiếm NB (quét QR, mã HS, tên NB) với bộ lọc trạng thái: Chờ tiếp nhận, Chờ lấy mẫu, Bỏ qua, Hủy mẫu
3. Chọn dịch vụ có trạng thái "Chờ tiếp nhận" → Nhấn tiếp nhận → chuyển sang "Chờ lấy mẫu"
4. Chọn dịch vụ "Chờ lấy mẫu" → Xác nhận lấy mẫu → chuyển sang "Đã lấy mẫu"

### Yêu cầu hoàn dịch vụ
Chỉ hoàn được dịch vụ có trạng thái Chờ tiếp nhận, Chờ lấy mẫu, Đã thanh toán (nguồn: XetNghiem.pdf).

## Thực hiện Sinh hóa - Huyết học

- **Có kết nối LIS**: Quét mã QR NB → Hệ thống đẩy thông tin qua LIS → LIS trả kết quả tự động (nguồn: XetNghiem.pdf)
- **Không kết nối LIS**: Chọn phòng, tìm NB, chọn dịch vụ "Đã lấy mẫu" → Tiếp nhận mẫu → Nhập kết quả thủ công

### Hủy tiếp nhận mẫu
Chỉ hủy được với dịch vụ có trạng thái "Đã tiếp nhận mẫu", đẩy thông tin hủy sang LIS (nguồn: XetNghiem.pdf).

## Thực hiện Giải phẫu bệnh - Vi sinh

Quy trình tương tự sinh hóa - huyết học, bổ sung các bước (nguồn: XetNghiem.pdf):
- Tiếp nhận mẫu
- Trả kết quả giải phẫu
- Duyệt kết quả
- In phiếu kết quả

## Các trạng thái dịch vụ xét nghiệm

Thứ tự các trạng thái (nguồn: XetNghiem.pdf):
1. **Chờ tiếp nhận** → (Tiếp nhận) → **Chờ lấy mẫu**
2. **Chờ lấy mẫu** → (Xác nhận lấy mẫu) → **Đã lấy mẫu**
3. **Đã lấy mẫu** → (Tiếp nhận mẫu) → **Đã tiếp nhận mẫu**
4. **Đã tiếp nhận mẫu** → (Có kết quả) → **Đã có KQ**
5. **Đã có KQ** → (Duyệt kết quả) → **Đã duyệt KQ**
6. **Đã lấy mẫu** → (Bỏ qua)
7. **Đã tiếp nhận mẫu** → (Hủy mẫu)

## Xem kết quả từ LIS

Dịch vụ có kết quả từ LIS trả về ở trạng thái "Đã có kết quả" hoặc "Đã duyệt kết quả", tùy theo thống nhất giữa bệnh viện và hệ thống (nguồn: XetNghiem.pdf).

## Phím tắt

- **F1**: Xem nhanh thông tin người bệnh (nguồn: XetNghiem.pdf)
- **F3**: Tìm mã dịch vụ
- **F6**: Quét mã người bệnh

## Xử lý tình huống

- **Không tìm thấy NB**: Chọn đúng phòng thực hiện, kiểm tra bộ lọc trạng thái (nguồn: XetNghiem.pdf)
- **Không thấy dịch vụ**: Trạng thái dịch vụ khác với bộ lọc đã chọn → chọn lại bộ lọc phù hợp

## Các trang liên quan

- [[kham-benh]]
- [[quy-trinh-kham-ngoai-tru]]
- [[chan-doan-hinh-anh-tdcn]]
- [[quan-ly-noi-tru]]