# Reflection Prompt Template

Template phỏng vấn cuối ngày — dùng cho Wild #9: Knowledge Interviewer.
AI sẽ chọn 3-5 câu hỏi từ danh sách dưới đây, xoay vòng để tránh nhàm chán.
**Không hỏi tất cả các câu mỗi ngày** — chọn tối đa 3 câu chính + 1 câu phụ.

---

## Bộ câu hỏi chính (chọn 2-3 câu mỗi ngày)

### Nhóm A: Học hỏi & Khám phá
1. **Hôm nay bạn học được điều gì mới nhất?** — Có thể là kỹ thuật, con người, quy trình, hoặc insight về thị trường.
2. **Có điều gì làm bạn ngạc nhiên hôm nay không?** — Điều gì khác với kỳ vọng của bạn?
3. **Có khái niệm/ý tưởng nào bạn muốn tìm hiểu sâu hơn không?** — Để AI có thể research sau.

### Nhóm B: Quyết định & Hành động
4. **Có quyết định nào bạn đưa ra hôm nay — lý do là gì?** — Focus vào WHY, không chỉ WHAT.
5. **Có quyết định nào bạn đã trì hoãn không? Tại sao?** — Phát hiện điểm nghẽn quyết định.
6. **Nếu được làm lại một việc hôm nay, bạn sẽ làm khác điều gì?** — Capture lesson learned ngay.

### Nhóm C: Vấn đề & Thách thức
7. **Có vấn đề nào chưa giải quyết được — bạn đã thử cách nào?** — Để AI có thể gợi ý hướng tiếp cận khác.
8. **Điều gì tốn nhiều thời gian nhất hôm nay?** — Xác định điểm kém hiệu quả.
9. **Có rủi ro nào mới xuất hiện mà bạn chưa lường trước không?** — Early warning system.

### Nhóm D: Thành công & Động lực
10. **Điều gì hoạt động tốt hơn bạn mong đợi?** — Ghi nhận thành công, dù nhỏ.
11. **Khoảnh khắc nào hôm nay bạn cảm thấy hiệu quả nhất?** — Xác định peak performance pattern.
12. **Có ai hoặc điều gì giúp bạn tiến bộ hôm nay không?** — Ghi nhận support system.

---

## Bộ câu hỏi phụ (chọn 1 câu, optional)

### Nhóm E: Meta & Cải thiện hệ thống
13. **Wiki có giúp ích gì cho công việc hôm nay không?** — Feedback loop cho hệ thống.
14. **Có thông tin nào bạn cần mà không tìm thấy trong wiki không?** — Gap analysis.
15. **Quy trình hiện tại có điểm nào cần cải thiện không?** — Process improvement.

---

## Cách AI sử dụng template này

### Bước 1: Chọn câu hỏi
- Xem ngày trong tuần để chọn nhóm phù hợp (đầu tuần → nhóm C, cuối tuần → nhóm D)
- Đảm bảo không lặp câu hỏi đã dùng 2 ngày liên tiếp
- Nếu hôm trước có vấn đề mở → ưu tiên hỏi follow-up (câu #7)

### Bước 2: Đặt câu hỏi tự nhiên
Không đọc nguyên văn template. Diễn đạt lại theo ngữ cảnh ngày hôm đó.
Ví dụ thay vì "Có quyết định nào bạn đưa ra hôm nay" → "Lúc chiều bạn nói đang phân vân giữa A và B — đã chốt chưa? Lý do bạn chọn là gì?"

### Bước 3: Lắng nghe và khai thác sâu
- Nếu câu trả lời có "WHY" thú vị → hỏi thêm 1 câu follow-up
- Nếu câu trả lời mơ hồ → yêu cầu ví dụ cụ thể
- Không ép nếu người dùng không muốn trả lời sâu

### Bước 4: Tổng hợp và lưu
- Tổng hợp thành mục có cấu trúc trong `_system/daily_reflection_log.md`
- Nếu phát hiện tri thức mới → đề xuất: "Lưu insight này vào wiki không? Trang nào phù hợp?"
- Nếu phát hiện vấn đề mở → ghi vào reflection log, tag để follow-up ngày mai

---

## Lịch xoay vòng gợi ý

| Ngày | Nhóm chính | Số câu hỏi | Ghi chú |
|---|---|---|---|
| Thứ Hai | B (Quyết định) + C (Vấn đề) | 3 | Focus tuần mới, xác định ưu tiên |
| Thứ Ba | A (Học hỏi) + B (Quyết định) | 3 | Deep work day |
| Thứ Tư | C (Vấn đề) + E (Meta) | 3 | Mid-week check-in |
| Thứ Năm | A (Học hỏi) + D (Thành công) | 3 | Năng lượng giảm → cần motivation |
| Thứ Sáu | D (Thành công) + E (Meta) | 4 | Tổng kết tuần, cải thiện hệ thống |
| Cuối tuần | Tự do — 2 câu nhẹ nhàng | 2 | Không ép, có thể skip |

---

## Anti-patterns (điều cần tránh)

- ❌ Hỏi tất cả 15 câu mỗi ngày — quá tải, gây nhàm chán
- ❌ Hỏi y hệt template không cá nhân hóa — mất kết nối
- ❌ Bỏ qua câu trả lời ngắn — đào sâu khi cần, nhưng tôn trọng khi người dùng muốn skip
- ❌ Không ghi lại gì sau reflection — mất toàn bộ giá trị
- ❌ Hỏi reflection khi người dùng đang vội/bận — có thể defer sang ngày hôm sau