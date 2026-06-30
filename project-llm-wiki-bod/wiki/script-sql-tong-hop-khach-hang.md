# Script SQL Tổng Hợp Khách Hàng

**Tóm tắt**: Các script SQL chuẩn để lấy số liệu tổng hợp và chi tiết đối soát cho 3 chỉ số khách hàng: Quay Lại, Tái Khám, Trung Thành.

**Nguồn**: `quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md`

**Cập nhật lần cuối**: 2026-06-02

---

## 1. Script Tổng Hợp Dashboard

### Mục đích

Báo cáo Dashboard Tổng hợp 3 chỉ số: Khách Quay Lại (Cross-sell), Khách Tái Khám (Follow-up), và Khách Trung Thành (Loyalty). Phục vụ Ban Giám đốc xem tỷ trọng từng nhóm trên tổng quy mô bệnh nhân trong tháng. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### Script SQL đầy đủ

```sql
/* 
======================================================================================
MỤC ĐÍCH (PURPOSE): 
- Báo cáo Dashboard Tổng hợp 3 chỉ số: Khách Quay Lại (Cross-sell), Khách Tái Khám (Follow-up), và Khách Trung Thành (Loyalty).
- Phục vụ Ban Giám đốc xem tỷ trọng từng nhóm trên tổng quy mô bệnh nhân trong tháng.

GIẢI THÍCH LOGIC:
1. Tính hạng đến viện chung (VisitRank) để tìm Khách quay lại (dùng dịch vụ mới).
2. Tìm khách dùng gói có chữ "tái khám" để đếm Khách tái khám.
3. Dùng hàm ROW_NUMBER() OVER(PARTITION BY MaBenhNhan, MaGoiDichVu) để đếm hạng gói (PkgRank). Nếu PkgRank >= 2 phát sinh trong tháng báo cáo -> Khách Trung Thành.
4. TẤT CẢ các chỉ số đều được bọc bởi COALESCE(TongTienSauTangGiam, TongTien) > 0 để lọc bỏ 0đ (trừ tệp Tái khám có thể có gói 0đ hợp lệ của Kế toán).
====================================================================================== 
*/

DECLARE @StartDate DATE = '2026-02-01';
DECLARE @EndDate DATE = '2026-05-01';

-- 1. Lấy khung dữ liệu tổng theo tháng (Mẫu số)
WITH BaseMonths AS (
    SELECT hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, hs.MaHoSo
    FROM HoSoKhamBenhNgoaiTru hs
    WHERE hs.NgayVaoKham >= @StartDate AND hs.NgayVaoKham < @EndDate
),

-- 2. Chuẩn bị dữ liệu Dịch vụ & Gói dịch vụ
BaseServices AS (
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, hs.MaHoSo,
        dv.MaGoiDichVu, gdv.TenGoiDichVu, dv.MaChiTieu, dm.TenChiTieu,
        COALESCE(dv.TongTienSauTangGiam, dv.TongTien) AS DoanhThuTinhToan 
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMDichVuChiTiet dm ON dv.MaChiTieu = dm.MaChiTieu
    LEFT JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
),

-- 3. Tệp Khách Tái Khám (Follow-up)
FollowUpCustomers AS (
    SELECT DISTINCT bs.MaBenhNhan, MONTH(bs.NgayKham) AS Thang, YEAR(bs.NgayKham) AS Nam
    FROM BaseServices bs
    WHERE bs.NgayKham >= @StartDate AND bs.NgayKham < @EndDate
      AND bs.TenGoiDichVu LIKE N'%tái khám%' 
),

-- 4. Tệp Khách Quay Lại (Dùng dịch vụ MỚI so với lần trước)
PatientRank AS (
    SELECT DISTINCT MaBenhNhan, CAST(NgayVaoKham AS DATE) AS NgayKham,
        DENSE_RANK() OVER (PARTITION BY MaBenhNhan ORDER BY CAST(NgayVaoKham AS DATE)) AS VisitRank
    FROM HoSoKhamBenhNgoaiTru
),
ReturningCustomers AS (
    SELECT DISTINCT dr.MaBenhNhan, MONTH(dr.NgayKham) AS Thang, YEAR(dr.NgayKham) AS Nam
    FROM PatientRank dr
    JOIN BaseServices cur ON dr.MaBenhNhan = cur.MaBenhNhan AND dr.NgayKham = cur.NgayKham
    JOIN PatientRank prev ON dr.MaBenhNhan = prev.MaBenhNhan AND prev.VisitRank = dr.VisitRank - 1
    WHERE dr.VisitRank >= 2
      AND dr.NgayKham >= @StartDate AND dr.NgayKham < @EndDate
      AND cur.DoanhThuTinhToan > 0 
      AND cur.TenGoiDichVu NOT LIKE N'%tái khám%'
      AND NOT EXISTS (
          SELECT 1 FROM BaseServices prev_srv
          WHERE prev_srv.MaBenhNhan = dr.MaBenhNhan AND prev_srv.NgayKham = prev.NgayKham AND prev_srv.MaChiTieu = cur.MaChiTieu
      )
),

-- 5. Tệp Khách Trung Thành (Lặp lại cùng 1 gói dịch vụ)
PackageUsageRank AS (
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, dv.MaGoiDichVu,
        ROW_NUMBER() OVER (PARTITION BY hs.MaBenhNhan, dv.MaGoiDichVu ORDER BY CAST(hs.NgayVaoKham AS DATE)) AS PkgRank
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
    WHERE gdv.TenGoiDichVu NOT LIKE N'%tái khám%' 
      AND COALESCE(dv.TongTienSauTangGiam, dv.TongTien) > 0
),
LoyalCustomers AS (
    SELECT DISTINCT MaBenhNhan, MONTH(NgayKham) AS Thang, YEAR(NgayKham) AS Nam
    FROM PackageUsageRank
    WHERE PkgRank >= 2
      AND NgayKham >= @StartDate AND NgayKham < @EndDate
)

-- TỔNG HỢP RA BÁO CÁO CUỐI CÙNG
SELECT 
    FORMAT(bm.NgayKham, 'MM/yyyy') AS [Tháng báo cáo],
    COUNT(bm.MaHoSo) AS [Tổng số lượt khám],
    COUNT(DISTINCT bm.MaBenhNhan) AS [Tổng bệnh nhân (duy nhất)],
    
    (SELECT COUNT(DISTINCT rc.MaBenhNhan) FROM ReturningCustomers rc WHERE rc.Thang = MONTH(bm.NgayKham) AND rc.Nam = YEAR(bm.NgayKham)) AS [Tổng số quay lại],
    (SELECT COUNT(DISTINCT fc.MaBenhNhan) FROM FollowUpCustomers fc WHERE fc.Thang = MONTH(bm.NgayKham) AND fc.Nam = YEAR(bm.NgayKham)) AS [Tổng số tái khám],
    (SELECT COUNT(DISTINCT lc.MaBenhNhan) FROM LoyalCustomers lc WHERE lc.Thang = MONTH(bm.NgayKham) AND lc.Nam = YEAR(bm.NgayKham)) AS [Tổng số trung thành],

    -- TÍNH TỶ LỆ % TỰ ĐỘNG
    ROUND(CAST((SELECT COUNT(DISTINCT rc.MaBenhNhan) FROM ReturningCustomers rc WHERE rc.Thang = MONTH(bm.NgayKham) AND rc.Nam = YEAR(bm.NgayKham)) AS FLOAT) / COUNT(DISTINCT bm.MaBenhNhan) * 100, 2) AS [Tỷ lệ quay lại (%)],
    ROUND(CAST((SELECT COUNT(DISTINCT fc.MaBenhNhan) FROM FollowUpCustomers fc WHERE fc.Thang = MONTH(bm.NgayKham) AND fc.Nam = YEAR(bm.NgayKham)) AS FLOAT) / COUNT(DISTINCT bm.MaBenhNhan) * 100, 2) AS [Tỷ lệ tái khám (%)],
    ROUND(CAST((SELECT COUNT(DISTINCT lc.MaBenhNhan) FROM LoyalCustomers lc WHERE lc.Thang = MONTH(bm.NgayKham) AND lc.Nam = YEAR(bm.NgayKham)) AS FLOAT) / COUNT(DISTINCT bm.MaBenhNhan) * 100, 2) AS [Tỷ lệ trung thành (%)]

FROM BaseMonths bm
GROUP BY MONTH(bm.NgayKham), YEAR(bm.NgayKham), FORMAT(bm.NgayKham, 'MM/yyyy')
ORDER BY YEAR(bm.NgayKham), MONTH(bm.NgayKham);
```

---

## 2. Script Chi Tiết Đối Soát

### Mục đích

Lấy chi tiết từng bệnh nhân thuộc từng phân loại để đối soát số liệu với Dashboard. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### Script SQL đầy đủ

```sql
DECLARE @StartDate DATE = '2026-03-01';
DECLARE @EndDate DATE = '2026-04-01'; -- Ví dụ lọc tháng 02/2026 (Từ 01/02 đến sát 01/03)
DECLARE @LoaiBaoCao VARCHAR(20) = 'TAI_KHAM'; -- Giao diện truyền vào 1 trong 3 biến: 'QUAY_LAI', 'TAI_KHAM', 'TRUNG_THANH'

WITH BaseServices AS (
    -- Tầng 1: Trích xuất giao dịch gốc, khóa chặt quy tắc tài chính
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, hs.MaHoSo,
        dv.MaGoiDichVu, gdv.TenGoiDichVu, dv.MaChiTieu, dm.TenChiTieu,
        COALESCE(dv.TongTienSauTangGiam, dv.TongTien) AS DoanhThuTinhToan 
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMDichVuChiTiet dm ON dv.MaChiTieu = dm.MaChiTieu
    LEFT JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
),
PatientRank AS (
    -- Hỗ trợ phân tích Khách Quay Lại
    SELECT DISTINCT MaBenhNhan, CAST(NgayVaoKham AS DATE) AS NgayKham,
        DENSE_RANK() OVER (PARTITION BY MaBenhNhan ORDER BY CAST(NgayVaoKham AS DATE)) AS VisitRank
    FROM HoSoKhamBenhNgoaiTru
),
PackageUsageRank AS (
    -- Hỗ trợ phân tích Khách Trung Thành
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, dv.MaGoiDichVu,
        ROW_NUMBER() OVER (PARTITION BY hs.MaBenhNhan, dv.MaGoiDichVu ORDER BY CAST(hs.NgayVaoKham AS DATE)) AS PkgRank
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
    WHERE gdv.TenGoiDichVu NOT LIKE N'%tái khám%' 
      AND COALESCE(dv.TongTienSauTangGiam, dv.TongTien) > 0
),
RawData AS (
    -- RẼ NHÁNH 1: CHI TIẾT KHÁCH QUAY LẠI (CROSS-SELL)
    SELECT 
        dr.MaBenhNhan, cur.NgayKham, cur.MaHoSo, cur.TenGoiDichVu, cur.TenChiTieu, cur.DoanhThuTinhToan,
        'QUAY_LAI' AS PhanLoai
    FROM PatientRank dr
    JOIN BaseServices cur ON dr.MaBenhNhan = cur.MaBenhNhan AND dr.NgayKham = cur.NgayKham
    JOIN PatientRank prev ON dr.MaBenhNhan = prev.MaBenhNhan AND prev.VisitRank = dr.VisitRank - 1
    WHERE dr.VisitRank >= 2
      AND dr.NgayKham >= @StartDate AND dr.NgayKham < @EndDate
      AND cur.DoanhThuTinhToan > 0 
      AND cur.TenGoiDichVu NOT LIKE N'%tái khám%'
      AND NOT EXISTS (
          SELECT 1 FROM BaseServices prev_srv
          WHERE prev_srv.MaBenhNhan = dr.MaBenhNhan AND prev_srv.NgayKham = prev.NgayKham AND prev_srv.MaChiTieu = cur.MaChiTieu
      )
      AND @LoaiBaoCao = 'QUAY_LAI'

    UNION ALL

    -- RẼ NHÁNH 2: CHI TIẾT KHÁCH TÁI KHÁM (FOLLOW-UP)
    SELECT 
        bs.MaBenhNhan, bs.NgayKham, bs.MaHoSo, bs.TenGoiDichVu, bs.TenChiTieu, bs.DoanhThuTinhToan,
        'TAI_KHAM' AS PhanLoai
    FROM BaseServices bs
    WHERE bs.NgayKham >= @StartDate AND bs.NgayKham < @EndDate
      AND bs.TenGoiDichVu LIKE N'%tái khám%' 
      AND @LoaiBaoCao = 'TAI_KHAM'

    UNION ALL

    -- RẼ NHÁNH 3: CHI TIẾT KHÁCH TRUNG THÀNH (LOYALTY)
    SELECT 
        bs.MaBenhNhan, pkr.NgayKham, bs.MaHoSo, bs.TenGoiDichVu, bs.TenChiTieu, bs.DoanhThuTinhToan,
        'TRUNG_THANH' AS PhanLoai
    FROM PackageUsageRank pkr
    JOIN BaseServices bs ON pkr.MaBenhNhan = bs.MaBenhNhan AND pkr.NgayKham = bs.NgayKham AND pkr.MaGoiDichVu = bs.MaGoiDichVu
    WHERE pkr.PkgRank >= 2
      AND pkr.NgayKham >= @StartDate AND pkr.NgayKham < @EndDate
      AND @LoaiBaoCao = 'TRUNG_THANH'
),
DeduplicatedData AS (
    -- CHỐT CHẶN KHỚP SỐ (CRITICAL FIX):
    -- Ép mỗi bệnh nhân chỉ xuất hiện đúng 1 dòng đại diện gần nhất trong tháng báo cáo.
    -- Đảm bảo số đếm dòng của kết quả = COUNT(DISTINCT MaBenhNhan) trên Dashboard.
    SELECT 
        rd.*,
        ROW_NUMBER() OVER(PARTITION BY rd.MaBenhNhan ORDER BY rd.NgayKham DESC, rd.DoanhThuTinhToan DESC) as RowNum
    FROM RawData rd
)
-- XUẤT RA GIAO DIỆN HIỂN THỊ CỦA BẢNG POPUP
SELECT 
    dd.MaBenhNhan AS [Mã Bệnh Nhân],
    bn.TenBenhNhan AS [Tên Bệnh Nhân],
    bn.SoDienThoai AS [Số Điện Thoại],
    dd.NgayKham AS [Ngày Khám Gần Nhất],
    ISNULL(dd.TenGoiDichVu, N'Dịch vụ lẻ') AS [Gói Dịch Vụ Tham Gia],
    dd.TenChiTieu AS [Chi Tiết Dịch Vụ],
    dd.DoanhThuTinhToan AS [Doanh Thu Ghi Nhận (VNĐ)]
FROM DeduplicatedData dd
LEFT JOIN DMBenhNhan bn ON dd.MaBenhNhan = bn.MaBenhNhan
WHERE dd.RowNum = 1
ORDER BY dd.NgayKham DESC;
```

---

## 3. Tham Số Đầu Vào

### Script Tổng Hợp

| Tham số | Kiểu dữ liệu | Mô tả |
|---------|--------------|-------|
| `@StartDate` | DATE | Ngày bắt đầu kỳ báo cáo |
| `@EndDate` | DATE | Ngày kết thúc kỳ báo cáo (không bao gồm) |

### Script Chi Tiết

| Tham số | Kiểu dữ liệu | Giá trị | Mô tả |
|---------|--------------|---------|-------|
| `@StartDate` | DATE | - | Ngày bắt đầu kỳ báo cáo |
| `@EndDate` | DATE | - | Ngày kết thúc kỳ báo cáo |
| `@LoaiBaoCao` | VARCHAR(20) | `QUAY_LAI`, `TAI_KHAM`, `TRUNG_THANH` | Loại báo cáo cần xuất |

---

## 4. Bảng Dữ Liệu Sử Dụng

| Bảng | Mô tả |
|------|-------|
| `HoSoKhamBenhNgoaiTru` | Hồ sơ khám bệnh ngoại trú |
| `ThuPhiDichVu` | Thu phí dịch vụ |
| `DMDichVuChiTiet` | Danh mục dịch vụ chi tiết |
| `DMGoiDichVu` | Danh mục gói dịch vụ |
| `DMBenhNhan` | Danh mục bệnh nhân |

---

## 5. Lưu Ý Kỹ Thuật

### Nguyên tắc COALESCE

Luôn sử dụng `COALESCE(TongTienSauTangGiam, TongTien)` để lấy đúng doanh thu thực thu. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### Nguyên tắc Khử Trùng

Mỗi bệnh nhân chỉ đếm 1 lần trong tháng bằng `COUNT(DISTINCT MaBenhNhan)` hoặc `ROW_NUMBER() ... WHERE RowNum = 1`. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

Xem chi tiết: [[quy-tac-ky-thuat-toan-cuc]]

---

## Các Trang Liên Quan

- [[quy-tac-tinh-khach-quay-lai]]
- [[quy-tac-tinh-khach-tai-kham]]
- [[khach-trung-thanh]]
- [[quy-tac-ky-thuat-toan-cuc]]
- [[dashboard-bao-cao-bod]]
