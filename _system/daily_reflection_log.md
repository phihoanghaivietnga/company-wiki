# Daily Reflection Log

Nhật ký tri thức hàng ngày — ghi lại bài học, quyết định, và tri thức mới từ các buổi phỏng vấn cuối ngày (Wild #9: Knowledge Interviewer).

---

## Cấu trúc một mục reflection

```markdown
## YYYY-MM-DD

**Ngày trong tuần**: Thứ X

**Bài học hôm nay**: 
- [tóm tắt điều mới học được hôm nay]

**Quyết định đã ra**: 
- [quyết định + lý do tại sao]

**Vấn đề đang mở**: 
- [vấn đề chưa giải quyết được + đã thử cách nào]

**Điều hoạt động tốt**: 
- [điều gì hoạt động tốt hơn mong đợi]

**Điều sẽ làm khác**: 
- [nếu làm lại hôm nay, sẽ thay đổi gì]

**Tri thức mới → wiki**: 
- [[ten-project/ten-trang-được-tạo]] — [mô tả ngắn]
- [[ten-project/ten-trang-được-cập-nhật]] — [nội dung cập nhật]

**Tâm trạng / Năng lượng**: [1-5] ⭐ — [ghi chú nếu cần]
```

---

## Danh sách reflection

## 2026-07-01

**Ngày trong tuần**: Thứ Tư

**Bài học hôm nay**: 
- Kim xử lý không nhất quán giữa 2 lần thực thi cùng 1 yêu cầu: Lần 1 (Daily Companion - Luồng 8) chỉ đọc index.md để đếm trang → bỏ sót note-rac.md (trang ngoài index); Lần 2 (Lint - Luồng 4) mới list toàn bộ thư mục wiki/ và đối chiếu với index → phát hiện đủ trang mồ côi + broken link. Gốc: Bước "quét nhanh" của Luồng 8 làm tắt, chưa list filesystem.
- Nhờ vậy mới biết điểm yếu của Kim để tiếp tục cải thiện.

**Quyết định đã ra**: 
- Xóa note-rac.md (trang test, không có nội dung thật)
- Xóa broken link `[[trang-khong-ton-tai]]` trong thong_tin_mau_thuan.md
- Sẽ tìm giải pháp sửa rule trong AGENTS_MASTER.md để đảm bảo Kim xử lý nhất quán, không làm tắt — cụ thể: bước quét của Luồng 8 cần phải list toàn bộ thư mục wiki/ + đối chiếu với index.md, không chỉ đọc index

**Vấn đề đang mở**: 
- Cần sửa AGENTS_MASTER.md — bổ sung bước kiểm tra chéo (list filesystem vs index) cho Luồng 8 để tránh bỏ sót trang mồ côi. Chưa chốt phương án cụ thể.

**Điều hoạt động tốt**: 
- Luồng 4 (Lint) phát hiện đầy đủ 3 vấn đề: 1 trang mồ côi, 1 broken link, 1 thiếu định dạng — checklist lint hoạt động chính xác

**Điều sẽ làm khác**: 
- Khi thực thi Luồng 8, phải list filesystem thay vì chỉ đọc index.md

**Tri thức mới → wiki**: 
- Không có trang mới được tạo/cập nhật từ reflection này

**Tâm trạng / Năng lượng**: 3 ⭐ — Phát hiện bug trong quy trình, cần sửa rule

---

---

## Ví dụ mẫu (template entry)

## 2026-06-29

**Ngày trong tuần**: Thứ Hai

**Bài học hôm nay**: 
- Context window của Qwen3 thực tế nhỏ hơn nhiều so với quảng cáo — cần chia nhỏ file input khi gửi prompt dài
- Sử dụng `_system/` làm nơi lưu trạng thái AI giúp agent không bị "mất trí nhớ" giữa các session

**Quyết định đã ra**: 
- Chuyển toàn bộ hệ thống GSD command từ slash-command cũ sang clinerules mới — vì slash-command format cũ không tương thích với Cline phiên bản mới

**Vấn đề đang mở**: 
- Wild #3 (DB connector) — chưa có thời gian thiết kế schema kết nối. Đã thử dùng SQLite local nhưng cần suy nghĩ thêm về sync.

**Điều hoạt động tốt**: 
- Format conflict audit log rõ ràng giúp tiết kiệm 30 phút mỗi lần review conflict

**Điều sẽ làm khác**: 
- Nên tạo template reflection sớm hơn thay vì đợi đến khi có PRD — reflection ngay từ đầu giúp capture được nhiều insight hơn

**Tri thức mới → wiki**: 
- [[_system/daily_reflection_log]] — Tạo mới file này để bắt đầu ghi nhận bài học hàng ngày

**Tâm trạng / Năng lượng**: 4 ⭐ — Đầu tuần năng lượng tốt, hoàn thành được nhiều việc

---

## Quy tắc quản lý log

1. **Mỗi ngày 1 mục** — AI thêm vào đầu file (mới nhất ở trên cùng).
2. **Nếu ngày không có reflection** (bỏ qua) → ghi: "Không có reflection — người dùng bỏ qua."
3. **Archive theo tháng**: Nếu file >15KB, tạo `daily_reflection_log_YYYY-MM.md` và bắt đầu file mới.
4. **AI chịu trách nhiệm** tổng hợp câu trả lời của người dùng thành các mục có cấu trúc — người dùng chỉ cần trả lời tự do, không cần format.