# 7 Chỉ Số Khách Hàng Chính

**Tóm tắt**: Định nghĩa phi kỹ thuật, ý nghĩa kinh doanh, đơn vị tính của 7 chỉ số hiển thị trên Dashboard báo cáo BOD.

**Nguồn**: `Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md`

**Cập nhật lần cuối**: 2026-05-29

---

## 1. Tổng Lượt Khám (Lũy Kế)

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Là tổng số lần đăng ký khám/chữa bệnh được thực hiện tại hệ thống. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Ví Dụ** | Nếu một khách hàng đến viện 3 lần trong kỳ lọc, hệ thống sẽ tính là 3 lượt khám. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Lượt đăng ký (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Quy mô tải vận hành tổng thể của bệnh viện. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |

**Chi tiết:** [[quy-tac-tinh-luot-kham]]

---

## 2. Khách Mới (Đến 1 Lần)

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Số lượng khách hàng thực hiện lượt khám đầu tiên trong hệ thống trong khoảng thời gian báo cáo. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Bệnh nhân (Unique) (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Hiệu quả của các chiến dịch tuyên truyền, quảng cáo Marketing. |

---

## 3. Khách Quay Lại (Đến 2 Lần)

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Số lượng khách hàng quay lại viện và phát sinh lượt khám thứ 2 trong hệ thống (lịch sử tính từ trước đến nay) nằm trong tháng báo cáo. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Bệnh nhân (Unique) (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Sức khỏe thương hiệu, lòng trung thành khách hàng, hiệu quả của chiến dịch Marketing và chất lượng dịch vụ tổng thể. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |

**Chi tiết:** [[quy-tac-tinh-khach-quay-lai]]

---

## 4. Khách VIP (Đến ≥ 3 Lần)

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Số lượng khách hàng trung thành, đã có tổng cộng từ 3 ngày đến viện khám trở lên trong toàn bộ lịch sử và có xuất hiện khám trong kỳ báo cáo. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Bệnh nhân (Unique) (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Lòng trung thành lâu dài của khách hàng, tỷ lệ giữ chân và tái khám lâu dài. |

---

## 5. Quản Trị Tái Khám

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Tỷ lệ phần trăm các ca phẫu thuật/thủ thuật được người bệnh tuân thủ quay lại tái khám kiểm tra trong vòng 1 tháng sau mổ. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Tỷ lệ phần trăm (%) (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Chất lượng tư vấn của bác sĩ, hiệu quả nhắc hẹn của điều dưỡng/CSKH, năng lực quản trị lâm sàng và tuân thủ điều trị. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |

**Chi tiết:** [[quy-tac-tinh-khach-tai-kham]]

---

## 6. Khách Chưa Quay Lại

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Danh sách hoặc số lượng bệnh nhân đã thực hiện phẫu thuật trong kỳ báo cáo nhưng quá hạn vẫn chưa thấy quay lại hệ thống để kiểm tra vết mổ. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Ca mổ / Bệnh nhân (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Danh sách cần can thiệp khẩn cấp để đảm bảo an toàn bệnh nhân sau phẫu thuật. |

---

## 7. Hộ Gia Đình Vàng (Lũy Kế)

| Thuộc tính | Nội dung |
|-----------|---------|
| **Ý Nghĩa Thực Tế** | Chỉ số gom nhóm các khách hàng có mối quan hệ thân nhân (dùng chung một số điện thoại đăng ký). (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Đơn Vị Tính** | Hộ gia đình (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |
| **Phản Ánh** | Khả năng khai thác dịch vụ theo mô hình gia đình - mỗi hộ gia đình mở ra tiềm năng phát triển vài bệnh nhân tiềm năng bên trong. (nguồn: Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md) |

---

## Tóm Tắt Bảng Tham Chiếu

| Chỉ Số | Đơn Vị | Ý Nghĩa Chính |
|-------|-------|-------------|
| Tổng Lượt Khám | Lượt đăng ký | Khối lượng vận hành |
| Khách Mới | Bệnh nhân (Unique) | Marketing hiệu quả |
| Khách Quay Lại | Bệnh nhân (Unique) | Lòng trung thành |
| Khách VIP | Bệnh nhân (Unique) | Trung thành lâu dài |
| Quản Trị Tái Khám | % | Tuân thủ lâm sàng |
| Khách Chưa Quay Lại | Ca/Bệnh nhân | Danh sách can thiệp |
| Hộ Gia Đình Vàng | Hộ gia đình | Tiềm năng khai thác |

---

## Các Trang Liên Quan

- [[dashboard-bao-cao-bod]]
- [[quy-tac-tinh-luot-kham]]
- [[quy-tac-tinh-khach-quay-lai]]
- [[quy-tac-tinh-khach-tai-kham]]
- [[ma-dich-vu-tai-kham]]
- [[chinh-sach-hop-nhat-tinh-toan]]
