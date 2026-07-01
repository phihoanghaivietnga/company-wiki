# YÊU CẦU: Bổ sung Luồng 13 — Chat Log Extraction vào company-wiki

Mày (Kim) thực hiện đúng 2 phần dưới đây, theo đúng thứ tự, không skip bước nào.
Đây là thay đổi vào chính hệ thống rule của mày, nên phải cẩn trọng tối đa —
không tự suy diễn thêm hành vi ngoài những gì được ghi rõ ở đây.

---

## PHẦN 1 — Sửa `AGENTS_MASTER.md`

### 1.1 Thêm vào bảng ánh xạ Trigger → Luồng

| Trigger | Luồng |
|---|---|
| "Đưa file chat log", "Tổng hợp chat này", "Extract vấn đề đã chốt" | Luồng 13 |

### 1.2 Thêm định nghĩa đầy đủ Luồng 13, giữ đúng văn phong tất định từng bước
như Luồng 4 đã sửa (KHÔNG viết kiểu checklist mô tả):

```markdown
## Luồng 13 — Chat Log Extraction

**Trigger:** "Đưa file chat log", "Tổng hợp chat này", "Extract vấn đề đã chốt"

**Input:** File nằm trong `project-X/raw/do/`, tên đúng format
`project-chat-YYYYMMDD.md` (ngày lấy trực tiếp từ tên file, không tự đoán).
Dòng đầu file có thể có `Nguồn: <cline|claude-web|chatgpt|khác>` (optional).
Nếu không có → khi trích dẫn ghi "nguồn: không rõ", không tự bịa.

Sai định dạng tên file, hoặc không nằm trong `raw/do/` của đúng 1 project →
DỪNG, báo lại user, không tự đoán/tự sửa tên file.

**Bước 1:** Đọc toàn bộ nội dung nguyên văn (không tóm tắt trước khi quét).

**Bước 2:** Quét sơ bộ toàn file, kiểm tra có đoạn nào RÕ RÀNG thuộc về project
khác (không phải project-X đang xử lý) không.
- Có → DỪNG TOÀN BỘ LUỒNG. Báo cho user: liệt kê đoạn nghi ngờ + lý do nghi
  ngờ, yêu cầu user sửa file gốc (xóa phần không liên quan) rồi chạy lại lệnh
  từ đầu. KHÔNG tự lọc bỏ, KHÔNG tiếp tục xử lý phần còn lại.
- Không có → qua Bước 3.

**Bước 3:** Quét và liệt kê candidate list. Mỗi candidate gồm:
- Vấn đề được nêu (1 câu)
- Trích đoạn gốc làm evidence (nguyên văn, ngắn, không diễn giải)
- Đánh giá sơ bộ: có vẻ đã chốt / có vẻ đang thảo luận (chỉ là gợi ý cho Bước
  4, KHÔNG tự quyết)

**Bước 4:** Trình bày toàn bộ candidate list theo đúng thứ tự xuất hiện trong
chat, hỏi xác nhận TỪNG item theo format:
- a) Đúng, đã chốt → ghi vào wiki
- b) Chưa chốt, chỉ thảo luận → ghi vào `wiki/pending-chat-issue.md`
- c) Đúng nhưng diễn giải sai → yêu cầu user cung cấp bản sửa, ghi theo bản sửa
- d) Không liên quan/bỏ qua hẳn, không ghi ở đâu cả

**Bước 5:** Với mọi item được đánh a) hoặc c) — trước khi ghi, kiểm tra có mâu
thuẫn với nội dung hiện có trong `wiki/` của project này không.
- Mâu thuẫn → áp dụng đúng Luồng xử lý Conflict hiện có (flag a/b/c/d riêng +
  bắt buộc ghi WHY trước khi update).
- Không mâu thuẫn → ghi thẳng.

**Bước 6:** Ghi các item a)/c) đã qua Bước 5 vào `wiki/` theo format chuẩn
hiện có, kèm dòng nguồn: `(nguồn: {tên file}, ngày {từ tên file})`.

**Bước 7:** Ghi các item b) vào `wiki/pending-chat-issue.md` (tạo file này nếu
project chưa có), mỗi dòng gồm: ngày phát hiện, nội dung, tên file nguồn. File
này được liệt lại mỗi khi Luồng 13 chạy tiếp cho project này.

**Bước 8:** Update `wiki/log.md`: "Ingest chat log {ngày}, chốt N/{tổng
candidate} vấn đề, M vào pending."

**Bước 9:** Move file gốc từ `raw/do/` sang `raw/done/` (move nguyên văn,
không sửa nội dung — đúng rule bất biến của `raw/`).
```

**Không cần sửa mục "Ranh giới ghi file"** — `wiki/` đã là thư mục được phép
ghi theo rule hiện có, `pending-chat-issue.md` nằm trong đó nên đã được bao
phủ sẵn. Không thêm rule ranh giới mới.

---

## PHẦN 2 — Sửa `OPERATIONS_GUIDE.md`

### 2.1 Thêm hướng dẫn vận hành Luồng 13 cho người vận hành (không phải cho AI
đọc — viết sao cho Phi hoặc bất kỳ ai đọc và làm theo được ngay, không cần hỏi
lại):
- Cách tạo file input đúng chuẩn: tên `project-chat-YYYYMMDD.md`, dòng đầu
  optional `Nguồn: ...`
- Lưu đúng vào đâu: `project-X/raw/do/` (path đầy đủ, ví dụ thật)
- Câu lệnh gõ để trigger
- Kỳ vọng Kim sẽ hỏi gì ở từng bước, trả lời a/b/c/d nghĩa là gì
- Trường hợp Kim báo "phát hiện nội dung lạc project" — người vận hành cần
  làm gì (sửa file gốc, chạy lại)
- Cách kiểm tra kết quả sau khi xong (file nào được tạo/sửa, ở đâu)

### 2.2 YÊU CẦU — bổ sung ví dụ Input/Output cho tất cả 13 luồng, theo 2 cách
khác nhau:

**Với 12 luồng đã có sẵn kịch bản test:** KHÔNG viết lại ví dụ mới. Thay vào
đó, với mỗi luồng, tìm đúng vị trí kịch bản test tương ứng trong
`KICH_BAN_TEST_12_LUONG.md` và thêm 1 dòng link/tham chiếu trong
OPERATIONS_GUIDE.md tới đúng vị trí đó (dùng heading anchor nếu file đó có
heading rõ ràng cho từng luồng).
- Nếu không xác định được chính xác vị trí kịch bản của luồng nào trong file
  đó (heading không rõ, hoặc không tìm thấy kịch bản tương ứng) → DỪNG, hỏi
  tao, KHÔNG tự đoán link hoặc tự viết ví dụ thay thế.

**Với Luồng 13 (mới, chưa có kịch bản test):** Viết ví dụ Input/Output trực
tiếp trong OPERATIONS_GUIDE.md, dựa đúng theo spec ở Phần 1.2 trên — KHÔNG bịa
hành vi ngoài spec.

---

## Ràng buộc thực hiện chung

1. Không sửa/xóa nội dung của các luồng khác ngoài phần được yêu cầu ở trên.
2. Giữ nguyên văn phong, cấu trúc heading hiện tại của cả 2 file.
3. Trước khi ghi đè file thật, in ra preview/diff phần sắp thêm để tao duyệt.
4. Không tự resolve bất kỳ điểm mờ nào — hỏi tao, đúng nguyên tắc conflict đã
   thống nhất từ đầu.