# Strategy Analysis Template

Template phân tích chiến lược — dùng cho Wild #4: Strategic Advisor.
Khi người dùng yêu cầu tư vấn ("Tư vấn về X", "Nên chọn A hay B", "Phân tích vấn đề Y"), AI sẽ điền template này để đưa ra phân tích có cấu trúc.

---

## Cấu trúc output

```markdown
## Phân tích chiến lược: "{vấn đề cần phân tích}"

**Ngày**: YYYY-MM-DD
**Người yêu cầu**: Người dùng
**Loại phân tích**: [Chọn phương án / Đánh giá rủi ro / Dự đoán xu hướng / Phân tích nguyên nhân gốc]

---

### 1. Bối cảnh (từ wiki)

[Tóm tắt thông tin liên quan từ wiki — chỉ dùng dữ liệu đã có, không bịa]

- Từ [[ten-project/trang-A]]: [thông tin liên quan 1]
- Từ [[ten-project/trang-B]]: [thông tin liên quan 2]
- Từ [[_global/trang-C]]: [thông tin liên quan 3]

---

### 2. Các góc nhìn / Phương án

| # | Góc nhìn / Phương án | Ưu điểm | Nhược điểm | Rủi ro | Nguồn tham khảo |
|---|---|---|---|---|---|
| 1 | Phương án A: [tên] | [ưu điểm 1], [ưu điểm 2] | [nhược điểm 1] | [rủi ro] | [[trang-1]] |
| 2 | Phương án B: [tên] | [ưu điểm 1], [ưu điểm 2] | [nhược điểm 1] | [rủi ro] | [[trang-2]] |
| 3 | Phương án C: [tên] | [ưu điểm 1] | [nhược điểm 1], [nhược điểm 2] | [rủi ro] | [[trang-3]] |

---

### 3. Phân tích trade-off

**Tiêu chí đánh giá** (dựa trên dữ liệu wiki và mục tiêu):
| Tiêu chí | Trọng số | Phương án A | Phương án B | Phương án C |
|---|---|---|---|---|
| [Tiêu chí 1 — ví dụ: Chi phí] | Cao | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| [Tiêu chí 2 — ví dụ: Tốc độ] | Trung bình | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| [Tiêu chí 3 — ví dụ: Rủi ro] | Cao | ⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Tổng** | — | **Điểm A** | **Điểm B** | **Điểm C** |

---

### 4. Khuyến nghị

**Đề xuất**: Chọn **Phương án [X]** 

**Lý do chính**:
1. [Lý do 1 — dựa trên dữ liệu]
2. [Lý do 2 — dựa trên dữ liệu]
3. [Lý do 3 — dựa trên dữ liệu]

**Điều kiện tiên quyết** (nếu có):
- [Điều kiện cần đáp ứng trước khi thực thi]

---

### 5. Rủi ro & Kế hoạch giảm thiểu

| Rủi ro | Mức độ | Xác suất | Giảm thiểu |
|---|---|---|---|
| [Rủi ro 1] | Cao / Trung bình / Thấp | ...% | [Cách giảm thiểu] |
| [Rủi ro 2] | ... | ...% | [Cách giảm thiểu] |

---

### 6. Bước tiếp theo

1. [Hành động cụ thể 1 — ai làm, deadline]
2. [Hành động cụ thể 2]
3. [Hành động cụ thể 3]

---

### 7. Độ tin cậy

**Mức độ**: Cao / Trung bình / Thấp

**Lý do**:
- [Số lượng nguồn dữ liệu đã tham khảo]
- [Chất lượng / độ fresh của dữ liệu]
- [Các giả định chưa kiểm chứng được]

> 💡 **Đề xuất**: Lưu phân tích này vào wiki không? (→ tạo/cập nhật trang liên quan)
```

---

## Ví dụ mẫu đã điền

## Phân tích chiến lược: "Nên chọn React Native hay Flutter cho mobile app mới?"

**Ngày**: 2026-06-29
**Người yêu cầu**: Người dùng
**Loại phân tích**: Chọn phương án

---

### 1. Bối cảnh (từ wiki)

- Từ [[project-sniper/tech-stack]]: Backend đã chọn Python FastAPI, team có 3 dev fullstack
- Từ [[project-sniper/product-requirements]]: App cần chạy trên iOS + Android, có module xử lý ảnh real-time
- Từ [[_global/team-skills]]: Team có 2 người biết React, 1 người biết Python (chưa ai biết Dart)

---

### 2. Các góc nhìn / Phương án

| # | Phương án | Ưu điểm | Nhược điểm | Rủi ro | Nguồn tham khảo |
|---|---|---|---|---|---|
| 1 | React Native | Team đã biết React (tiết kiệm 2-4 tuần học); Hệ sinh thái JS lớn; Nhiều thư viện xử lý ảnh | Performance kém hơn Flutter cho real-time; Cần bridge native cho module ảnh | Performance issue với real-time image processing | [[project-sniper/tech-stack]] |
| 2 | Flutter | Performance tốt hơn (compile to native); UI nhất quán iOS/Android; Xử lý ảnh tốt qua dart:ui | Team phải học Dart (2-4 tuần); Hệ sinh thái nhỏ hơn RN | Learning curve — delay dự án | [[_global/mobile-benchmark]] |
| 3 | Native (Swift + Kotlin) | Performance tối ưu nhất; Full access native API | Tốn gấp đôi effort (2 platform); Team phải học 2 ngôn ngữ mới | Budget & timeline không khả thi | [[_global/mobile-benchmark]] |

---

### 3. Phân tích trade-off

| Tiêu chí | Trọng số | React Native | Flutter | Native |
|---|---|---|---|---|
| Time-to-market | Cao | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| Performance (xử lý ảnh) | Cao | ⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ |
| Learning curve | Trung bình | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| Long-term maintainability | Trung bình | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| **Tổng** | — | **10** | **10** | **7** |

---

### 4. Khuyến nghị

**Đề xuất**: Chọn **Flutter**

**Lý do chính**:
1. Module xử lý ảnh real-time là tính năng cốt lõi — Flutter vượt trội về performance cho use case này
2. Mặc dù team phải học Dart, nhưng về lâu dài Flutter có DX (Developer Experience) tốt hơn và ít bug layout hơn RN
3. Flutter đang chiếm thị phần mobile tăng trưởng nhanh — tương lai có nhiều thư viện và dev hơn

**Điều kiện tiên quyết**:
- Dành 2 tuần đầu sprint cho team học Dart + Flutter fundamentals
- Build prototype xử lý ảnh real-time trong tuần 3 để xác nhận performance

---

### 5. Rủi ro & Kế hoạch giảm thiểu

| Rủi ro | Mức độ | Xác suất | Giảm thiểu |
|---|---|---|---|
| Team học Dart chậm hơn dự kiến | Trung bình | 30% | Assign 1 dev React làm trước 1 tuần, sau đó mentor 2 người còn lại |
| Performance Flutter không đạt với xử lý ảnh real-time | Cao | 20% | Build prototype sớm (tuần 3) — nếu fail → fallback sang React Native với native module |
| Thiếu Flutter dev để hire sau này | Thấp | 15% | Đào tạo nội bộ + document kỹ trong wiki |

---

### 6. Bước tiếp theo

1. [Tuần 1] Setup Flutter environment + course nội bộ — Tech Lead
2. [Tuần 2-3] Build prototype xử lý ảnh real-time — Senior Dev
3. [Tuần 4] Quyết định go/no-go dựa trên prototype kết quả — Cả team + PO

---

### 7. Độ tin cậy

**Mức độ**: Trung bình

**Lý do**:
- Dữ liệu benchmark từ 3 nguồn — nhưng tất cả đều từ 2025 (cần cập nhật 2026)
- Chưa có dữ liệu thực tế từ team (chưa ai test Flutter thực tế với use case này)
- Giả định: Flutter performance cho real-time image processing dựa trên benchmark chung, chưa test với dataset thật

> 💡 **Đề xuất**: Lưu phân tích này vào wiki không? → [[project-sniper/mobile-tech-decision]]

---

## Quy tắc sử dụng template

1. **Chỉ dùng khi được yêu cầu** — trigger: "Tư vấn...", "Nên chọn...", "Phân tích...", "So sánh A vs B"
2. **Không bịa dữ liệu** — nếu wiki không có thông tin, ghi rõ "Chưa có dữ liệu trong wiki về vấn đề này"
3. **Luôn hỏi lưu vào wiki** — sau khi phân tích xong, đề xuất lưu thành trang wiki
4. **Cập nhật độ tin cậy** — đánh giá trung thực, không tâng bốc
5. **Nếu vấn đề đơn giản** (không cần phân tích sâu) → trả lời trực tiếp, không dùng template này