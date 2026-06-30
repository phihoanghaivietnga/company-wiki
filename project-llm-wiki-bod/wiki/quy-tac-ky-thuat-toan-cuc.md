# Quy Tắc Kỹ Thuật Toàn Cục

**Tóm tắt**: Các quy tắc kỹ thuật bắt buộc áp dụng cho toàn bộ script SQL báo cáo, đảm bảo số liệu chính xác và tránh sai số nhập liệu.

**Nguồn**: `quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md`

**Cập nhật lần cuối**: 2026-06-02

---

## Tổng Quan

Để đảm bảo số liệu không bị Kế toán bóc phốt và tránh sai số do việc nhập liệu của Lễ tân, toàn bộ các script SQL phục vụ báo cáo phải tuân thủ 2 nguyên tắc ngầm định. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

---

## 1. Nguyên Tắc Dòng Tiền (COALESCE Rule)

### Mô tả

Doanh thu tính toán luôn phải sử dụng hàm `COALESCE(ThuPhiDichVu.TongTienSauTangGiam, ThuPhiDichVu.TongTien)`. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### Logic

```
Nếu có giảm giá/voucher → lấy tiền sau giảm
Nếu không → lấy tiền gốc
```

### Công thức SQL

```sql
COALESCE(dv.TongTienSauTangGiam, dv.TongTien) AS DoanhThuTinhToan
```

### Tại sao cần quy tắc này?

- Tránh sai số khi Lễ tân nhập giảm giá/voucher nhưng vẫn tính trên tiền gốc
- Đảm bảo doanh thu thực thu đúng với số tiền bệnh nhân thanh toán
- Tránh tranh chấp với Kế toán khi đối soát

### Áp dụng cho chỉ số nào?

- **Bắt buộc** cho: [[quy-tac-tinh-khach-quay-lai]], [[khach-trung-thanh]]
- **Không áp dụng** cho: [[quy-tac-tinh-khach-tai-kham]] (chấp nhận giao dịch 0 VNĐ)

---

## 2. Nguyên Tắc Khử Trùng (Deduplication Rule)

### Mô tả

Trên báo cáo tổng hợp theo Tháng, mỗi `MaBenhNhan` chỉ được đếm là **1 (Unique)** cho dù họ có đến viện thỏa mãn điều kiện đó N lần trong tháng. (nguồn: quy_tac_quay_lai_tai_kham_trung_thanh_20260602.md)

### Mục đích

- Tránh đếm trùng một khách hàng nhiều lần
- Đảm bảo số liệu "Tổng bệnh nhân" khớp với `COUNT(DISTINCT MaBenhNhan)`
- Thống nhất phương pháp tính toán giữa các báo cáo

### Công thức SQL

#### Cách 1: Sử dụng DISTINCT trong COUNT

```sql
COUNT(DISTINCT MaBenhNhan) AS [Tổng bệnh nhân duy nhất]
```

#### Cách 2: Sử dụng ROW_NUMBER() cho chi tiết đối soát

```sql
DeduplicatedData AS (
    SELECT 
        rd.*,
        ROW_NUMBER() OVER(PARTITION BY rd.MaBenhNhan ORDER BY rd.NgayKham DESC, rd.DoanhThuTinhToan DESC) as RowNum
    FROM RawData rd
)
-- Sau đó lọc WHERE RowNum = 1
```

### Ví dụ minh họa

**Tình huống:** Bệnh nhân A đến viện 3 lần trong tháng 5/2026:
- Ngày 5/5: Khám dịch vụ B (mới) → Thỏa mãn "Khách Quay Lại"
- Ngày 15/5: Khám dịch vụ C (mới) → Thỏa mãn "Khách Quay Lại"
- Ngày 25/5: Dùng gói "Tái khám" → Thỏa mãn "Khách Tái Khám"

**Kết quả đếm tháng 5/2026:**
- Khách Quay Lại: **1** (chỉ đếm bệnh nhân A một lần)
- Khách Tái Khám: **1** (chỉ đếm bệnh nhân A một lần)

---

## Bảng Tóm Tắt Áp Dụng

| Chỉ số | COALESCE Rule | Deduplication Rule |
|--------|---------------|-------------------|
| [[quy-tac-tinh-khach-quay-lai]] | ✅ Bắt buộc (> 0) | ✅ Bắt buộc |
| [[quy-tac-tinh-khach-tai-kham]] | ❌ Không (chấp nhận 0đ) | ✅ Bắt buộc |
| [[khach-trung-thanh]] | ✅ Bắt buộc (> 0) | ✅ Bắt buộc |

---

## Script SQL Mẫu

### Base Services với COALESCE

```sql
BaseServices AS (
    SELECT 
        hs.MaBenhNhan, CAST(hs.NgayVaoKham AS DATE) AS NgayKham, hs.MaHoSo,
        dv.MaGoiDichVu, gdv.TenGoiDichVu, dv.MaChiTieu, dm.TenChiTieu,
        COALESCE(dv.TongTienSauTangGiam, dv.TongTien) AS DoanhThuTinhToan 
    FROM HoSoKhamBenhNgoaiTru hs
    INNER JOIN ThuPhiDichVu dv ON hs.MaHoSo = dv.MaHoSo AND dv.DaDongTien = 1
    INNER JOIN DMDichVuChiTiet dm ON dv.MaChiTieu = dm.MaChiTieu
    LEFT JOIN DMGoiDichVu gdv ON dv.MaGoiDichVu = gdv.MaGoiDichVu
)
```

### Khử trùng cho báo cáo chi tiết

```sql
-- Ép mỗi bệnh nhân chỉ xuất hiện đúng 1 dòng đại diện gần nhất trong tháng báo cáo
-- Đảm bảo số đếm dòng của kết quả = COUNT(DISTINCT MaBenhNhan) trên Dashboard
DeduplicatedData AS (
    SELECT 
        rd.*,
        ROW_NUMBER() OVER(PARTITION BY rd.MaBenhNhan ORDER BY rd.NgayKham DESC, rd.DoanhThuTinhToan DESC) as RowNum
    FROM RawData rd
)
SELECT * FROM DeduplicatedData WHERE RowNum = 1
```

---

## Các Trang Liên Quan

- [[quy-tac-tinh-khach-quay-lai]]
- [[quy-tac-tinh-khach-tai-kham]]
- [[khach-trung-thanh]]
- [[script-sql-tong-hop-khach-hang]]
- [[chinh-sach-hop-nhat-tinh-toan]]
