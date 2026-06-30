---
title: Quy Tắc Phân Loại Hành Vi Khách Hàng (Quay Lại, Tái Khám, Trung Thành)
author: Bộ phận Phân tích Dữ liệu (Data Analytics)
date: 2026-06-02
tags: [customer_segmentation, data_dictionary, HIS_rules, business_logic]
status: APPROVED (Chốt nghiệp vụ liên phòng ban)
---

# HỆ THỐNG PHÂN LOẠI HÀNH VI KHÁCH HÀNG TRÊN HIS

Tài liệu này định nghĩa các quy tắc cốt lõi để phân loại khách hàng, làm cơ sở tính toán cho toàn bộ Dashboard và Báo cáo quản trị của Bệnh viện. Mỗi phân loại phục vụ một mục tiêu quản trị riêng biệt và được sở hữu (own) bởi các phòng ban khác nhau.

## 1. KHÁCH QUAY LẠI (CROSS-SELL CUSTOMERS)
Đo lường năng lực bán chéo dịch vụ mới cho tệp khách hàng cũ.

* **Phòng ban chốt nghiệp vụ:** Phòng Marketing.
* **Mục tiêu:** Đánh giá hiệu quả của đội ngũ Sale/CSKH trong việc tư vấn dịch vụ mới.
* **Điều kiện hạt dữ liệu (Data Grain Conditions):**
    1.  **Lịch sử:** Phải là lần đến viện từ thứ 2 trở lên tính trên toàn bộ lịch sử (`VisitRank >= 2`).
    2.  **Hành vi:** Tại lần khám này, phải phát sinh **Ít nhất 01 Dịch Vụ Chi Tiết (`MaChiTieu`) MỚI HOÀN TOÀN** so với tập dịch vụ đã sử dụng ở lần khám liền kề trước đó.
    3.  **Tài chính:** Phải phát sinh doanh thu thực tế (Tổng tiền > 0).
* **Điều kiện loại trừ (Exclusions):**
    * Bị loại bỏ nếu chỉ lặp lại y xì đúc các dịch vụ cũ.
    * Bị loại bỏ nếu sử dụng gói dịch vụ có chữ "Tái khám".

## 2. KHÁCH TÁI KHÁM (FOLLOW-UP CUSTOMERS)
Đo lường tính tuân thủ phác đồ điều trị và trách nhiệm lâm sàng.

* **Phòng ban chốt nghiệp vụ:** Phòng Kế toán & Phòng Marketing.
* **Mục tiêu:** Kiểm soát tỷ lệ bệnh nhân quay lại kiểm tra tình trạng bệnh/phẫu thuật theo đúng chỉ định của Bác sĩ.
* **Điều kiện hạt dữ liệu (Data Grain Conditions):**
    1.  **Hành vi:** Hồ sơ khám bệnh có phát sinh Gói Dịch Vụ chứa cụm từ khóa **"Tái khám"** (`TenGoiDichVu LIKE N'%tái khám%'`).
    2.  **Tài chính:** Chấp nhận cả các giao dịch 0 VNĐ (Miễn phí bảo hành/Tái khám theo gói đã thu tiền trước đó).
* **Điều kiện loại trừ (Exclusions):** * Không phụ thuộc vào hạng khám (`VisitRank` có thể = 1 nếu hệ thống cũ không ghi nhận ca mổ).

## 3. KHÁCH TRUNG THÀNH (LOYAL CUSTOMERS)
Đo lường niềm tin thương hiệu và giá trị vòng đời khách hàng (CLV) đối với các dịch vụ cốt lõi.

* **Phòng ban chốt nghiệp vụ:** Phòng CNTT (Data Analytics).
* **Mục tiêu:** Nhận diện tệp Khách hàng VIP, mỏ vàng của bệnh viện và phát hiện sớm rủi ro rời bỏ (Churn) đối với các gói dịch vụ mạn tính/định kỳ.
* **Điều kiện hạt dữ liệu (Data Grain Conditions):**
    1.  **Hành vi:** Khách hàng sử dụng **LẶP LẠI CÙNG 01 GÓI DỊCH VỤ** từ 2 lần trở lên (Tính theo `PackageRank >= 2` trên từng ID Bệnh nhân và ID Gói).
    2.  **Tài chính:** Bắt buộc phải phát sinh doanh thu thực tế (Tổng tiền > 0). Người dùng chịu chi trả tiền túi nhiều lần cho cùng một dịch vụ mới được tính là trung thành.
* **Điều kiện loại trừ (Exclusions):**
    * Tuyệt đối loại bỏ các gói thuộc nhóm "Tái khám" lâm sàng bắt buộc.

---

## 4. CHỐT CHẶN KỸ THUẬT TOÀN CỤC (GLOBAL TECHNICAL RULES)

Để đảm bảo số liệu không bị Kế toán bóc phốt và tránh sai số do việc nhập liệu của Lễ tân, toàn bộ các script SQL phục vụ báo cáo phải tuân thủ 2 nguyên tắc ngầm định:

1.  **Nguyên tắc Dòng tiền (COALESCE Rule):** Doanh thu tính toán luôn phải sử dụng hàm `COALESCE(ThuPhiDichVu.TongTienSauTangGiam, ThuPhiDichVu.TongTien)`. Nếu có giảm giá/voucher, lấy tiền sau giảm. Nếu không, lấy tiền gốc. Mốc `> 0` được áp dụng khắt khe cho tệp Quay Lại và Trung Thành.
2.  **Nguyên tắc Khử Trùng (Deduplication Rule):** Trên báo cáo tổng hợp theo Tháng, mỗi `MaBenhNhan` chỉ được đếm là **1 (Unique)** cho dù họ có đến viện thỏa mãn điều kiện đó N lần trong tháng.


## 5. Các script sql để lấy số liệu tổng hợp và chi tiết đối soát

### 5.1 script lấy số liệu tổng hợp
```SQL
/* 
======================================================================================
MỤC ĐÍCH (PURPOSE): 
- Báo cáo Dashboard Tổng hợp 3 chỉ số: Khách Quay Lại (Cross-sell), Khách Tái Khám (Follow-up), và Khách Trung Thành (Loyalty).
- Phục vụ Ban Giám đốc xem tỷ trọng từng nhóm trên tổng quy mô bệnh nhân trong tháng.

GIẢI THÍCH LOGIC CHO AI (AI LOGIC EXPLANATION):
1. Tính hạng đến viện chung (VisitRank) để tìm Khách quay lại (dùng dịch vụ mới).
2. Tìm khách dùng gói có chữ "tái khám" để đếm Khách tái khám.
3. [MỚI] Dùng hàm ROW_NUMBER() OVER(PARTITION BY MaBenhNhan, MaGoiDichVu) để đếm hạng gói (PkgRank). Nếu PkgRank >= 2 phát sinh trong tháng báo cáo -> Khách Trung Thành.
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


### 5.2 script lấy số liệu chi tiết để đối soát
```SQL
DECLARE @StartDate DATE = '2026-03-01';
DECLARE @EndDate DATE = '2026-04-01'; -- Ví dụ lọc tháng 02/2026 (Từ 01/02 đến sát 01/03)
DECLARE @LoaiBaoCao VARCHAR(20) = 'TAI_KHAM'; -- Giao diện truyền vào 1 trong 3 biến: 'QUAY_LAI', 'TAI_KHAM', 'TRUNG_THANH'

-- Tong Lượt khám và Tổng khách hàng duy nhất, khi cần lấy chi tiết thì bỏ count đi
-- select count(*) LuotKham, count(distinct MaBenhNhan) TongKhachHang_DuyNhat  from HoSoKhamBenhNgoaiTru where NgayVaoKham >= @StartDate and NgayVaoKham < @EndDate;

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