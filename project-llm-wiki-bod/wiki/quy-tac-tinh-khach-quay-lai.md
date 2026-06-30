# Quy Tắc Tính Khách Quay Lại (Cross-Sell)

**Tóm tắt**: Định nghĩa "Khách Quay Lại" - khách hàng đến lần 2 trở đi VÀ sử dụng dịch vụ mới so với lần trước, đo lường năng lực bán chéo (cross-sell) của đội ngũ Sale/CSKH.

**Nguồn**: `quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md`, `Tai_Lieu_Nghiep_Vu_Bao_Cao_BOD_v2.md`

**Cập nhật lần cuối**: 2026-06-02

---

## Định Nghĩa

"Khách Quay Lại" là khách hàng đến viện từ lần thứ 2 trở đi (trên toàn bộ lịch sử) VÀ tại lần khám đó phát sinh **ít nhất một dịch vụ chi tiết mới hoàn toàn** so với lần khám liền kề trước đó. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Ý Nghĩa Kinh Doanh

Chỉ số này đo lường **năng lực bán chéo dịch vụ mới (cross-sell)** cho tệp khách hàng cũ. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md) Nó đánh giá hiệu quả của đội ngũ Sale/CSKH trong việc tư vấn dịch vụ mới cho khách hàng đã từng sử dụng dịch vụ tại bệnh viện.

## Phòng Ban Sở Hữu

**Phòng Marketing** - đơn vị chốt nghiệp vụ và chịu trách nhiệm về chỉ số này. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Đơn Vị Tính

- Bệnh nhân (Unique) – **Đếm mỗi bệnh nhân chỉ một lần** trong tháng báo cáo (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Điều Kiện Hạt Dữ Liệu (Data Grain Conditions)

### 1. Điều kiện Lịch sử

Phải là lần đến viện từ thứ 2 trở lên tính trên toàn bộ lịch sử (`VisitRank >= 2`). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

**Ví dụ:** Bệnh nhân B từng khám Mắt tại viện vào năm 2023. Đến tháng 5/2026, BN B quay lại khám. Hệ thống ghi nhận lịch sử năm 2023 và xếp BN B vào tệp "Khách Quay Lại" (nếu thỏa mãn các điều kiện khác).

### 2. Điều kiện Hành vi

Tại lần khám này, phải phát sinh **ít nhất một Dịch Vụ Chi Tiết (`MaChiTieu`) MỚI HOÀN TOÀN** so với tập dịch vụ đã sử dụng ở lần khám liền kề trước đó. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

**Ví dụ:** 
- Lần 1: Khách dùng dịch vụ A, B, C
- Lần 2: Khách dùng dịch vụ A, B, D → **Được tính** (có dịch vụ D mới)
- Lần 3: Khách dùng dịch vụ A, B, C → **Không được tính** (lặp y xì dịch vụ cũ)

### 3. Điều kiện Tài chính

Phải phát sinh doanh thu thực tế (Tổng tiền > 0). (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

## Điều Kiện Loại Trừ (Exclusions)

Khách hàng bị loại khỏi tệp "Khách Quay Lại" nếu: (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

1. **Chỉ lặp lại y xì các dịch vụ cũ** - không có dịch vụ mới nào
2. **Sử dụng gói dịch vụ có chữ "Tái khám"** - các gói này thuộc phân loại [[quy-tac-tinh-khach-tai-kham]]

## Phân Biệt với Các Chỉ Số Khác

### So với Khách Tái Khám

- **Khách Quay Lại** = Khách dùng dịch vụ MỚI, đo lường năng lực bán chéo
- **Khách Tái Khám** = Khách dùng gói "Tái khám", đo lường tuân thủ điều trị

Một khách hàng có thể được đếm ở cả hai tệp nếu họ vừa dùng dịch vụ mới VÀ dùng gói tái khám trong cùng lần đến. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### So với Khách Trung Thành

- **Khách Quay Lại** = Dùng dịch vụ MỚI (khác với lần trước)
- **Khách Trung Thành** = Lặp lại CÙNG một gói dịch vụ từ 2 lần trở lên

Xem thêm: [[khach-trung-thanh]]

## Script SQL Tham Khảo

### Tính hạng đến viện (VisitRank)

```sql
-- Tính hạng đến viện để xác định khách quay lại
PatientRank AS (
    SELECT DISTINCT MaBenhNhan, CAST(NgayVaoKham AS DATE) AS NgayKham,
        DENSE_RANK() OVER (PARTITION BY MaBenhNhan ORDER BY CAST(NgayVaoKham AS DATE)) AS VisitRank
    FROM HoSoKhamBenhNgoaiTru
)
```

### Lấy danh sách Khách Quay Lại

```sql
ReturningCustomers AS (
    SELECT DISTINCT dr.MaBenhNhan, MONTH(dr.NgayKham) AS Thang, YEAR(dr.NgayKham) AS Nam
    FROM PatientRank dr
    JOIN BaseServices cur ON dr.MaBenhNhan = cur.MaBenhNhan AND dr.NgayKham = cur.NgayKham
    JOIN PatientRank prev ON dr.MaBenhNhan = prev.MaBenhNhan AND prev.VisitRank = dr.VisitRank - 1
    WHERE dr.VisitRank >= 2
      AND cur.DoanhThuTinhToan > 0 
      AND cur.TenGoiDichVu NOT LIKE N'%tái khám%'
      AND NOT EXISTS (
          SELECT 1 FROM BaseServices prev_srv
          WHERE prev_srv.MaBenhNhan = dr.MaBenhNhan 
            AND prev_srv.NgayKham = prev.NgayKham 
            AND prev_srv.MaChiTieu = cur.MaChiTieu
      )
)
```

Script đầy đủ xem tại: [[script-sql-tong-hop-khach-hang]]

## Các Trang Liên Quan

- [[quy-tac-tinh-khach-tai-kham]]
- [[khach-trung-thanh]]
- [[quy-tac-ky-thuat-toan-cuc]]
- [[chi-so-khach-hang]]
- [[script-sql-tong-hop-khach-hang]]
