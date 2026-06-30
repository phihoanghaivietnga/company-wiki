# Chính Sách Hợp Nhất Tính Toán

**Tóm tắt**: Tóm tắt và giải quyết 5 bài toán tính toán chính yếu cần được thống nhất trong hệ thống báo cáo BOD.

**Nguồn**: `Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md`

**Cập nhật lần cuối**: 2026-05-29

---

## Bài Toán 1: Phạm Vi Định Nghĩa "Khách Mới" vs "Khách Quay Lại"

### Vấn Đề Đặt Ra
Hệ thống cần xác định: Khi khách hàng đến khám, xét dịch vụ sử dụng **so với toàn bộ lịch sử** hay **so với lần khám liền trước**? (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Quyết Định
**Tính trên toàn bộ lịch sử hoạt động của hệ thống.** (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Giải Thích
Cách tính này giữ nguyên nhất quán với [[quy-tac-tinh-khach-quay-lai]] và cho phép theo dõi lòng trung thành khách hàng từ lần đầu tiên họ ghé thăm bệnh viện.

---

## Bài Toán 2: Xử Lý Trùng Lặp Khi Lọc Lũy Kế Nhiều Tháng

### Vấn Đề Đặt Ra
Khi Lãnh đạo xem dữ liệu nhiều tháng (Lũy kế), bệnh nhân có thể bị đếm nhiều lần ở các tệp khác nhau nếu thứ hạng của họ thay đổi qua các tháng. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

**Ví dụ Vấn Đề:** Bệnh nhân A mổ Phaco tháng 1 (Khách Mới). Tháng 6 quay lại cắt kính. Lọc "Từ Tháng 1 đến Tháng 6" → hệ thống hiển thị BN A **2 lần**: 1 lần ở "Khách Mới" + 1 lần ở "Khách Quay lại". (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Quyết Định
**Chỉ xét trạng thái (hạng) cuối cùng của bệnh nhân tại ngày kết thúc của dải thời gian lọc.** (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Giải Thích
Với dải lọc "Từ Tháng 1 đến Tháng 6", hệ thống chỉ chốt thứ hạng của BN A vào cuối tháng 6. BN A được đếm **01 lần duy nhất** ở tệp "Khách Quay lại" (thứ hạng cuối cùng của anh ta).

---

## Bài Toán 3: Phân Nhóm Chi Tiết Thời Gian Tái Khám

### Vấn Đề Đặt Ra
Hiện tại giao diện gom tất cả các ca tái khám thành một nhóm chung là "Đã khám (≤ 1 tháng)". Có cần chia cụ thể các mốc ngày không? (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Quyết Định
**Duy trì cách phân nhóm hiện tại.** Trên giao diện báo cáo hiện tại đang được gom chung thành nhóm **"Đã khám (≤ 1 tháng)"** và **"Khám sau 1 tháng"**. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Giải Thích
Mục đích là đơn giản hóa hiển thị và tập trung vào hai câu hỏi chính:
- Bệnh nhân có tái khám đúng hạn (≤ 1 tháng) không?
- Bệnh nhân có tái khám muộn hơn quy định không?

---

## Bài Toán 4: Xử Lý Lượt Kiểm Tra Cùng Ngày Phẫu Thuật

### Vấn Đề Đặt Ra
Bộ lọc đang tự động loại bỏ các lượt khám cùng ngày mổ. Có cần giữ logic này không? (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Quyết Định
**Loại trừ hoàn toàn.** Lượt khám/kiểm tra phát sinh cùng ngày mổ không được tính là tái khám. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Giải Thích
"Tuân thủ tái khám" phải phản ánh **hành động chủ động quay lại viện vào các ngày tiếp theo** (Khoảng cách ≥ 1 ngày), không phải những lần kiểm tra cận lâm sàng ngay trong cùng phiên phẫu thuật.

---

## Bài Toán 5: Phân Biệt "Khám Kiểm Tra Sau PT" vs "Khám Bệnh Mới"

### Vấn Đề Đặt Ra
Khi bệnh nhân quay lại viện sau phẫu thuật:
- Nếu không phát sinh phí → Tính là "Đã tái khám"?
- Nếu phát sinh phí dịch vụ → Tính là "Khám bệnh mới hoàn toàn"? (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Quyết Định
**Dựa trên danh sách mã dịch vụ cụ thể.** Chỉ những lần khám có **tên dịch vụ chứa từ "Tái khám" hoặc "Khám lại"** được tính vào [[quy-tac-tinh-khach-tai-kham]]. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

### Giải Thích

Từ trao đổi với Phòng Kế Toán: (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md)

- Kế toán **không quản lý** tiêu chí "Khách quay lại" theo góc độ giữ chân khách hàng (lòng trung thành)
- Kế toán chỉ theo dõi "Khách tái khám" dựa trên bộ lọc là **9 mã dịch vụ cụ thể**:
  - 3 dịch vụ **có thu phí** (Tái khám cho bệnh nhân viêm)
  - 6 dịch vụ **miễn phí** (Tái khám sau phẫu thuật các loại)

Xem danh sách chi tiết tại: [[ma-dich-vu-tai-kham]]

---

## Tóm Tắt Quyết Định

| Bài Toán | Quyết Định |
|----------|-----------|
| Định nghĩa Khách Mới/Quay Lại | Tính trên toàn bộ lịch sử |
| Xử lý Lũy kế | Chỉ xét trạng thái cuối cùng |
| Phân nhóm thời gian tái khám | Giữ nguyên: ≤1 tháng / >1 tháng |
| Lượt khám cùng ngày mổ | Loại trừ hoàn toàn |
| Khám tái khám vs khám bệnh mới | Dựa trên mã dịch vụ cụ thể |

---

## Các Trang Liên Quan

- [[quy-tac-tinh-luot-kham]]
- [[quy-tac-tinh-khach-tai-kham]]
- [[quy-tac-tinh-khach-quay-lai]]
- [[ma-dich-vu-tai-kham]]
- [[chi-so-khach-hang]]
