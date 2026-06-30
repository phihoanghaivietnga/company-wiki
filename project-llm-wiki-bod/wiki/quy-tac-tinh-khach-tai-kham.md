# Quy Tắc Tính Khách Tái Khám (Follow-Up)

**Tóm tắt**: Định nghĩa "Khách Tái Khám" - khách hàng sử dụng gói dịch vụ có chứa từ "Tái khám", đo lường tính tuân thủ phác đồ điều trị và trách nhiệm lâm sàng.

**Nguồn**: `quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md`, `Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md`

**Cập nhật lần cuối**: 2026-06-02

---

## Định Nghĩa

"Khách Tái Khám" là khách hàng có hồ sơ khám bệnh phát sinh **Gói Dịch Vụ chứa cụm từ khóa "Tái khám"** (`TenGoiDichVu LIKE N'%tái khám%'`). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Ý Nghĩa Kinh Doanh

Chỉ số này đo lường **tính tuân thủ phác đồ điều trị và trách nhiệm lâm sàng**. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md) Nó kiểm soát tỷ lệ bệnh nhân quay lại kiểm tra tình trạng bệnh/phẫu thuật theo đúng chỉ định của Bác sĩ.

## Phòng Ban Sở Hữu

**Phòng Kế toán & Phòng Marketing** - các đơn vị chốt nghiệp vụ và chịu trách nhiệm về chỉ số này. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Đơn Vị Tính

- Bệnh nhân (Unique) – **Đếm mỗi bệnh nhân chỉ một lần** trong tháng báo cáo (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Điều Kiện Hạt Dữ Liệu (Data Grain Conditions)

### 1. Điều kiện Hành vi

Hồ sơ khám bệnh có phát sinh Gói Dịch Vụ chứa cụm từ khóa **"Tái khám"**. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

```sql
TenGoiDichVu LIKE N'%tái khám%'
```

Xem danh sách đầy đủ tại: [[ma-dich-vu-tai-kham]]

### 2. Điều kiện Tài chính

**Chấp nhận cả các giao dịch 0 VNĐ** (Miễn phí bảo hành/Tái khám theo gói đã thu tiền trước đó). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

Đây là điểm khác biệt quan trọng với các chỉ số khác - khách tái khám có thể không phát sinh doanh thu mới nhưng vẫn được tính.

## Điều Kiện Loại Trừ (Exclusions)

Không phụ thuộc vào hạng khám (`VisitRank` có thể = 1 nếu hệ thống cũ không ghi nhận ca mổ). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

**Lưu ý:** Khách tái khám được tính độc lập với lần đến thứ mấy trong lịch sử bệnh nhân.

## Phân Biệt với Các Chỉ Số Khác

### So với Khách Quay Lại

- **Khách Tái Khám** = Dùng gói có chữ "Tái khám", đo lường tuân thủ điều trị
- **Khách Quay Lại** = Dùng dịch vụ MỚI (khác với lần trước), đo lường bán chéo

Xem thêm: [[quy-tac-tinh-khach-quay-lai]]

### So với Khách Trung Thành

- **Khách Tái Khám** = Gói "Tái khám" (lâm sàng bắt buộc)
- **Khách Trung Thành** = Lặp lại CÙNG một gói dịch vụ từ 2 lần trở lên (loại trừ gói tái khám)

Xem thêm: [[khach-trung-thanh]]

## Script SQL Tham Khảo

### Lấy danh sách Khách Tái Khám

```sql
-- Tệp Khách Tái Khám (Follow-up)
FollowUpCustomers AS (
    SELECT DISTINCT bs.MaBenhNhan, MONTH(bs.NgayKham) AS Thang, YEAR(bs.NgayKham) AS Nam
    FROM BaseServices bs
    WHERE bs.TenGoiDichVu LIKE N'%tái khám%' 
)
```

### Lấy chi tiết đối soát

```sql
-- Chi tiết Khách Tái Khám
SELECT 
    bs.MaBenhNhan, bs.NgayKham, bs.MaHoSo, 
    bs.TenGoiDichVu, bs.TenChiTieu, bs.DoanhThuTinhToan
FROM BaseServices bs
WHERE bs.TenGoiDichVu LIKE N'%tái khám%'
```

Script đầy đủ xem tại: [[script-sql-tong-hop-khach-hang]]

## Các Trang Liên Quan

- [[ma-dich-vu-tai-kham]]
- [[quy-tac-tinh-khach-quay-lai]]
- [[khach-trung-thanh]]
- [[quy-tac-ky-thuat-toan-cuc]]
- [[chi-so-khach-hang]]
- [[script-sql-tong-hop-khach-hang]]
