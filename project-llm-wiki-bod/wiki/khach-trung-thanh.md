# Khách Trung Thành (Loyalty)

**Tóm tắt**: Định nghĩa "Khách Trung Thành" - khách hàng sử dụng lặp lại cùng một gói dịch vụ từ 2 lần trở lên, đo lường niềm tin thương hiệu và giá trị vòng đời khách hàng (CLV).

**Nguồn**: `quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md`

**Cập nhật lần cuối**: 2026-06-02

---

## Định Nghĩa

"Khách Trung Thành" là khách hàng sử dụng **LẶP LẠI CÙNG MỘT GÓI DỊCH VỤ** từ 2 lần trở lên (Tính theo `PackageRank >= 2` trên từng ID Bệnh nhân và ID Gói). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Ý Nghĩa Kinh Doanh

Chỉ số này đo lường **niềm tin thương hiệu và giá trị vòng đời khách hàng (CLV)** đối với các dịch vụ cốt lõi. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md) 

Nó giúp:
- Nhận diện tệp **Khách hàng VIP** - mỏ vàng của bệnh viện
- Phát hiện sớm **rủi ro rời bỏ (Churn)** đối với các gói dịch vụ mạn tính/định kỳ

## Phòng Ban Sở Hữu

**Phòng CNTT (Data Analytics)** - đơn vị chốt nghiệp vụ và chịu trách nhiệm về chỉ số này. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Đơn Vị Tính

- Bệnh nhân (Unique) – **Đếm mỗi bệnh nhân chỉ một lần** trong tháng báo cáo (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Điều Kiện Hạt Dữ Liệu (Data Grain Conditions)

### 1. Điều kiện Hành vi

Khách hàng sử dụng **lặp lại CÙNG một gói dịch vụ** từ 2 lần trở lên. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

**Công thức:** `PackageRank >= 2` trên từng `MaBenhNhan` và `MaGoiDichVu`

**Ví dụ:**
- Khách A dùng gói "Gói khám mắt cơ bản" lần 1 → PackageRank = 1
- Khách A dùng gói "Gói khám mắt cơ bản" lần 2 → PackageRank = 2 → **Được tính là Khách Trung Thành**

### 2. Điều kiện Tài chính

**Bắt buộc phải phát sinh doanh thu thực tế** (Tổng tiền > 0). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

Người dùng chịu chi trả tiền túi nhiều lần cho cùng một dịch vụ mới được tính là trung thành.

## Điều Kiện Loại Trừ (Exclusions)

**Tuyệt đối loại bỏ các gói thuộc nhóm "Tái khám"** lâm sàng bắt buộc. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

Lý do: Gói tái khám là nghĩa vụ y khoa bắt buộc, không phản ánh lòng trung thành tự nguyện của khách hàng.

## Phân Biệt với Các Chỉ Số Khác

### So với Khách Quay Lại

| | Khách Trung Thành | Khách Quay Lại |
|---|---|---|
| **Hành vi** | Lặp lại CÙNG một gói | Dùng dịch vụ MỚI |
| **Ý nghĩa** | Lòng trung thành thương hiệu | Năng lực bán chéo (cross-sell) |
| **Điều kiện** | PackageRank >= 2 | VisitRank >= 2 + dịch vụ mới |

Xem thêm: [[quy-tac-tinh-khach-quay-lai]]

### So với Khách Tái Khám

| | Khách Trung Thành | Khách Tái Khám |
|---|---|---|
| **Hành vi** | Lặp lại gói bất kỳ (trừ tái khám) | Dùng gói có chữ "Tái khám" |
| **Ý nghĩa** | Lòng trung thành tự nguyện | Tuân thủ phác đồ điều trị |
| **Gói tái khám** | Loại trừ | Bao gồm |

Xem thêm: [[quy-tac-tinh-khach-tai-kham]]

## Script SQL Tham Khảo

### Tính hạng gói (PackageRank)

```sql
-- Tính hạng gói để xác định khách trung thành
PackageUsageRank AS (
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, dv.MaGoiDichVu,
        ROW_NUMBER() OVER (PARTITION BY hs.MaBenhNhan, dv.MaGoiDichVu ORDER BY CAST(hs.NgayVaoKham AS DATE)) AS PkgRank
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
    WHERE gdv.TenGoiDichVu NOT LIKE N'%tái khám%' 
      AND COALESCE(dv.TongTienSauTangGiam, dv.TongTien) > 0
)
```

### Lấy danh sách Khách Trung Thành

```sql
LoyalCustomers AS (
    SELECT DISTINCT MaBenhNhan, MONTH(NgayKham) AS Thang, YEAR(NgayKham) AS Nam
    FROM PackageUsageRank
    WHERE PkgRank >= 2
)
```

Script đầy đủ xem tại: [[script-sql-tong-hop-khach-hang]]

## Ứng Dụng Quản Trị

### Nhận diện Khách hàng VIP

Khách trung thành với gói dịch vụ cao cấp là tệp khách hàng quan trọng nhất cần:
- Chăm sóc đặc biệt (CSKH ưu tiên)
- Ưu đãi riêng (loyalty program)
- Theo dõi sát sao để tránh rời bỏ

### Phát hiện rủi ro Churn

Nếu khách trung thành ngừng sử dụng gói dịch vụ:
- Cảnh báo sớm cho team CSKH
- Triển khai chiến dịch giữ chân
- Phân tích nguyên nhân rời bỏ

## Các Trang Liên Quan

- [[quy-tac-tinh-khach-quay-lai]]
- [[quy-tac-tinh-khach-tai-kham]]
- [[quy-tac-ky-thuat-toan-cuc]]
- [[chi-so-khach-hang]]
- [[script-sql-tong-hop-khach-hang]]
