# Hướng Dẫn Manual Runner - Chạy Thủ Công

**Tóm tắt**: Manual Runner là giao diện web cho phép người dùng chạy Loader thủ công từ browser, lựa chọn bảng cụ thể, khoảng ngày, và xem kết quả thực thời. Hỗ trợ chạy Dimension full-load hoặc Fact incremental đơn lẻ.

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (GEM_CODE_MAP.md, PROJECT_CHRONICLE.md sections)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Truy Cập Manual Runner

### URL
```
http://localhost:8501/

hoặc

http://[YOUR_SERVER_IP]:8501/manual-runner
```

### Điều Kiện Tiên Quyết
- ETL Service đang chạy (`python main.py`)
- `.env` chứa đầy đủ connection string
- `config/tables.yaml` đã cấu hình facility + bảng

---

## 2. Giao Diện Chính

```
┌─────────────────────────────────────────────────────────┐
│  Manual Runner - Chạy ETL Thủ Công                     │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [Chế Độ Chạy]                                         │
│  ☑ Chạy Dimension (Full-load)  ☐ Chạy Fact (Incremental)│
│                                                         │
│  [Chọn Facility]                                        │
│  Dropdown: [hanoi ▼]  (Hà Nội)                         │
│                                                         │
│  [Chọn Bảng]                                            │
│  Dropdown: [DimBenhNhan ▼]                             │
│                                                         │
│  [Khoảng Thời Gian]  (Chỉ cho Fact)                    │
│  Từ ngày: [2026-05-20] ↓                               │
│  Đến ngày: [2026-05-22] ↓                              │
│                                                         │
│  [▶ Chạy Ngay]  [■ Dừng]  [↻ Reset]                    │
│                                                         │
│  ────────────────────────────────────────────────────  │
│  [Tiến Độ]                                             │
│  ████████░░░░░░░░░ 45% (Đang nạp Landing...)          │
│                                                         │
│  [Log Real-time]                                        │
│  2026-05-22 14:30:45.123 [hanoi] [START] Nạp DimBN... │
│  2026-05-22 14:30:48.456 [hanoi] [INFO] Nạp 50k rows  │
│  2026-05-22 14:30:52.789 [hanoi] [SUCCESS] Hoàn tất   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 3. Chế Độ Chạy: Dimension vs Fact

### Dimension (Full-load)

**Chế Độ**: Nạp toàn bộ danh mục từ Production

**Luồng**:
```
Production DMBenhNhan
    ↓ (SELECT toàn bộ)
ODS Facility (TRUNCATE + nạp)
    ↓ (MERGE)
Datamart
```

**Bảng Hỗ Trợ**:
- DimBenhNhan
- DimBenh
- DimLoaiGoiDichVu
- DimDichVu (đặc biệt: 3 bảng tuần tự)

**Thời Gian Thực Hiện**: 
- Bảng nhỏ: 10-30 giây
- DimBenhNhan: 2-5 phút

(nguồn: GEM_CODE_MAP.md)

### Fact (Incremental)

**Chế Độ**: Nạp dữ liệu phát sinh theo khoảng ngày (D-3 cửa sổ)

**Luồng**:
```
Production (từ ngày - 3 đến ngày)
    ↓ (SELECT điều kiện WHERE)
Landing (TRUNCATE + nạp)
    ↓ (MERGE hard delete D-3)
ODS Facility
    ↓ (MERGE hard delete D-3)
Datamart
```

**Bảng Hỗ Trợ**:
- ThuPhiDichVu
- ThuPhiBaoHiem
- ThuPhiTangGiam
- ThuPhiGoi
- DoThiLuc
- HoSoKhamBenhNgoaiTru

**Lưu Ý**: Chọn "ThuPhiDichVu" → tự động nạp cả 3 bảng doanh thu (ThuPhiDichVu, ThuPhiBaoHiem, ThuPhiTangGiam)

**Thời Gian Thực Hiện**:
- 1 ngày: 30-60 giây
- 1 tuần: 2-5 phút

(nguồn: GEM_CODE_MAP.md - ADR-39, ADR-40)

---

## 4. Hướng Dẫn Từng Bước

### Bước 1: Chọn Chế Độ
```
Chế độ Dimension (Full-load)
☑ Chạy Dimension
☐ Chạy Fact
```

### Bước 2: Chọn Facility
```
Facility: [hanoi ▼]
(Cơ sở Hà Nội)
```

### Bước 3: Chọn Bảng

**Nếu Dimension**:
```
Bảng: [DimBenhNhan ▼]
```

**Nếu Fact**:
```
Bảng: [ThuPhiDichVu ▼]
(Ghi chú: Sẽ tự động chạy 3 bảng)
```

### Bước 4: Nhập Khoảng Ngày (Chỉ Fact)
```
Từ ngày: [2026-05-20]
Đến ngày: [2026-05-22]

→ Sẽ lấy dữ liệu từ 2026-05-17 (D-3) đến 2026-05-22
```

### Bước 5: Chạy
```
[▶ Chạy Ngay]
```

### Bước 6: Theo Dõi Log
```
[Tiến Độ]
████████░░░░░░░░░ 45%

[Log Real-time]
2026-05-22 14:30:45.123 [hanoi] [DimensionLoader] [START]
2026-05-22 14:30:50.456 [hanoi] [DimensionLoader] [INFO] 50000 rows
2026-05-22 14:30:55.789 [hanoi] [DimensionLoader] [SUCCESS]
```

---

## 5. Các Tình Huống Sử Dụng

### Tình Huống A: Cập Nhật Danh Mục Bệnh Nhân

**Bối Cảnh**: Vừa nhập thêm 10k bệnh nhân mới vào Production, cần sync ngay

**Các Bước**:
1. Chọn: ☑ Dimension
2. Facility: [hanoi]
3. Bảng: [DimBenhNhan]
4. Nhấn: [▶ Chạy Ngay]
5. Chờ 3-5 phút, log thấy [SUCCESS] → OK

**Kết Quả**: 10k bệnh nhân mới đã có trong ODS + Datamart

(nguồn: [[dimension-loader]])

### Tình Huống B: Đối Chiếu Doanh Thu Hôm Qua

**Bối Cảnh**: Hôm qua 2026-05-21 có doanh thu phát sinh, cần load để kiểm tra

**Các Bước**:
1. Chọn: ☐ Fact (incremental)
2. Facility: [hanoi]
3. Bảng: [ThuPhiDichVu]
4. Từ ngày: [2026-05-21]
5. Đến ngày: [2026-05-21]
6. Nhấn: [▶ Chạy Ngay]
7. Chờ 1-2 phút → thấy [SUCCESS]

**Kết Quả**: 3 bảng doanh thu (ThuPhi, ThuPhiBH, ThuPhiTG) đều được load để kiểm tra đối chiếu

(nguồn: [[fact-loader]] - ADR-40)

### Tình Huống C: Khắc Phục Lỗi - Chạy Lại Bảng Cụ Thể

**Bối Cảnh**: Chạy Auto lúc sáng bị lỗi Fact ThuPhiGoi, không cần chạy lại Dimension

**Các Bước**:
1. Chọn: ☐ Fact (incremental)
2. Facility: [hanoi]
3. Bảng: [ThuPhiGoi]
4. Từ ngày: [2026-05-22] (hôm nay)
5. Đến ngày: [2026-05-22]
6. Nhấn: [▶ Chạy Ngay]
7. Theo dõi log

**Lợi Ích**: Chạy riêng bảng lỗi, không ảnh hưởng Dimension/Fact khác

(nguồn: [[fact-loader]] - ADR-39)

---

## 6. Xử Lý Tình Huống Lỗi

### Trường Hợp: Chạy Bị Lỗi, Log Hiển Thị ERROR

```
2026-05-22 14:35:00 [hanoi] [FactLoader] [ERROR] TRUNCATE failed
2026-05-22 14:35:01 [hanoi] [FactLoader] [ERROR] Process stopped
```

**Cách Xử Lý**:
1. Nhấn: [■ Dừng] để hủy tiến trình
2. Chờ 10 giây để DB clear lock
3. Nhấn: [↻ Reset] để reset form
4. Chạy lại: [▶ Chạy Ngay]

### Trường Hợp: Chạy Quá Lâu, Bị Timeout

```
[Tiến Độ]
████░░░░░░░░░░░░░░ 20% (Quá 10 phút)
```

**Cách Xử Lý**:
1. Nhấn: [■ Dừng]
2. Kiểm tra:
   - Production DB có lag không?
   - Network bình thường không?
   - Server đó tải cao không?
3. Sau 5 phút: [↻ Reset] + [▶ Chạy Ngay] lại

(nguồn: [[loi-va-khac-phuc]])

---

## 7. Quy Tắc Sử Dụng

| Quy Tắc | Chi Tiết |
|---|---|
| **Chỉ 1 Lần Chạy** | Không bấm [▶ Chạy Ngay] 2 lần liên tiếp, chờ [SUCCESS] mới bấm lại |
| **Khoảng Ngày Fact** | Không nhập khoảng > 1 tháng (sẽ quá lâu) |
| **Đôi Khi Chạy Dim** | Không cần chạy Dimension hàng ngày, thường 1-2 lần tuần |
| **Log Detail** | Nếu lỗi, copy log gửi cho admin để debug |

---

## 8. Kết Quả Thành Công

**Dấu Hiệu Chạy OK**:
```
✓ Log hiển thị [SUCCESS] ở cuối
✓ Tiến độ 100%
✓ Số row nạp hợp lý (không 0, không quá lớn)
✓ Từ [START] đến [SUCCESS]: < 5 phút

Ví dụ:
[14:30:45.123] [hanoi] [DimensionLoader] [START] DimBenhNhan...
[14:30:50.456] [hanoi] [DimensionLoader] [INFO] TRUNCATE OK
[14:30:52.789] [hanoi] [DimensionLoader] [INFO] Nạp 150000 rows
[14:30:55.000] [hanoi] [DimensionLoader] [SUCCESS] MERGE OK
```

---

## Các Trang Liên Quan

- [[huong-dan-auto-runner]] - Chạy tự động qua Orchestrator
- [[fact-loader]] - Chi tiết nạp Fact
- [[dimension-loader]] - Chi tiết nạp Dimension
- [[loi-va-khac-phuc]] - Debug lỗi phổ biến
