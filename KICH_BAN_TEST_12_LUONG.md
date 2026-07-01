# Kịch bản Test 12 Luồng — Kim (company-wiki)

Mục tiêu: làm quen tay với từng luồng bằng dữ liệu giả lập, không đụng vào project thật.
Làm theo đúng thứ tự — các luồng sau dùng lại dữ liệu luồng trước tạo ra.

---

## Giai đoạn 0 — Setup project test

**Gõ:**
```
Tạo project mới tên là test-kim
```

**Kỳ vọng:** Kim tạo `project-test-kim/` với `AGENTS.md`, `raw/do/`, `raw/done/`, `wiki/index.md`, `wiki/log.md`, hỏi mày điền mô tả project. Điền tạm: *"Project sandbox để test 12 luồng của Kim"*.

**Check nhanh:** vào thư mục xem đủ cấu trúc chưa trước khi qua bước tiếp.

---

## Giai đoạn 1 — Core: Scan → Ingest → Conflict → Feedback Loop → Lint

### Test 1 — Luồng 1: Daily Scan

**Chuẩn bị:** Tạo file `project-test-kim/raw/do/A-lich-release.md` với nội dung:

```markdown
# Lịch release sản phẩm Sniper

Sản phẩm Sniper dự kiến release vào Q3/2026.
Team quyết định ưu tiên tính năng push notification trước paywall.
```

**Gõ:**
```
Scan project-test-kim
```

**Kỳ vọng:** Bảng kết quả scan liệt kê đúng 1 file `A-lich-release.md`, hỏi mày có muốn ingest không. **Chưa tự ingest** — nếu nó tự ingest luôn là sai quy trình (Luồng 1 Bước 2 bắt buộc chờ xác nhận).

---

### Test 2 — Luồng 2: Ingest

**Gõ:**
```
Ingest tất cả
```

**Kỳ vọng:**
- Kim hiện bảng phân loại (thuộc project nào, loại tri thức, trang wiki nào bị ảnh hưởng) — hỏi xác nhận trước khi ghi
- Sau xác nhận: tạo trang wiki mới (ví dụ `release-schedule.md`), cập nhật `wiki/index.md` + `wiki/log.md`
- File `A-lich-release.md` chuyển từ `raw/do/` sang `raw/done/`, có header YAML metadata (`ingest_date`, `wiki_pages_affected`, `status: active`)

**Check:** mở file trong `raw/done/` xem có đúng header không — đây là chi tiết hay bị bỏ sót.

---

### Test 3 — Luồng 3 + Luồng Xử lý Conflict + Luồng 10: Conflict Feedback Loop

**Chuẩn bị:** Tạo file `project-test-kim/raw/do/B-lich-release-update.md` với nội dung **cố tình mâu thuẫn** với file A:

```markdown
# Cập nhật lịch release Sniper

Do delay từ đối tác thanh toán, Sniper dời release sang Q4/2026.
Ưu tiên paywall trước, push notification làm sau.
```

**Gõ:**
```
Ingest project-test-kim
```

**Kỳ vọng:** Ingest **dừng lại giữa chừng**, hiện đúng format:
```
⚠️ PHÁT HIỆN CONFLICT — Cần xem xét
...
(a) Có — B thay thế hoàn toàn A
(b) Có một phần
(c) Không — cả hai đều đúng
(d) Không chắc
```

**Bạn trả lời:** `a` (B thay thế A — vì dữ liệu mới hơn)

**Kỳ vọng tiếp:** Kim hỏi ngay *"Lý do bạn chọn phương án này là gì?"* — **đây là chỗ dễ lộ lỗi nhất**, nếu nó bỏ qua câu hỏi WHY và tự động ghi wiki luôn là sai.

**Trả lời lý do:** *"Vì B là thông tin mới nhất, đã xác nhận với đối tác thanh toán"*

**Kỳ vọng cuối:**
- Trang `release-schedule.md` được update, đoạn cũ đánh dấu `[LỖI THỜI]` (gạch ngang, không xóa)
- `_system/conflict_audit_log.md` có entry mới, trạng thái `resolved`, có ghi đủ lý do
- Kim tự báo: *"Conflict #00X đã giải quyết. Không phát sinh conflict mới"* (cascade check chạy)

---

### Test 4 — Luồng 4: Lint/Audit

**Chuẩn bị:** Vào `project-test-kim/wiki/`, tự tay sửa 1 trang, thêm link hỏng: `[[trang-khong-ton-tai]]`. Đồng thời tạo 1 trang mồ côi mới không link từ đâu cả, ví dụ `wiki/note-rac.md` với nội dung bất kỳ.

**Gõ:**
```
Lint project-test-kim
```

**Kỳ vọng:** Báo cáo phát hiện đúng cả 2 lỗi (broken link + trang mồ côi), có đề xuất fix cho từng cái. Nếu chỉ báo 1 trong 2 → lint chưa quét đủ checklist 7 mục trong spec.

---

## Giai đoạn 2 — Trí tuệ: Search, Reasoning, Advisor

*Cần ít nhất 2-3 trang wiki đã có (từ Giai đoạn 1) mới test có ý nghĩa.*

### Test 5 — Luồng 5: Contextual Search

**Gõ:**
```
Tìm tất cả thông tin về lịch release Sniper
```

**Kỳ vọng:** Kim liệt kê đúng trang `release-schedule.md`, trích đoạn liên quan, ghi rõ nguồn (A.md/B.md), và **phải nhắc đến việc Q3 đã lỗi thời, Q4 là hiện tại** — nếu nó chỉ trả lời chung chung không phân biệt cũ/mới là chưa đọc kỹ wiki.

---

### Test 6 — Luồng 6: Local Reasoning

**Gõ:**
```
Tại sao Sniper lại đổi thứ tự ưu tiên tính năng?
```

**Kỳ vọng:** Đây là câu hỏi **không có sẵn câu trả lời trực tiếp** trong wiki (phải suy luận từ việc dời lịch → đổi ưu tiên). Kim phải trình bày "chuỗi suy luận" từng bước (theo format Luồng 6), không phải chỉ trích dẫn nguyên văn.

---

### Test 7 — Luồng 7: Strategic Advisor

**Gõ:**
```
Nên ưu tiên paywall hay push notification trước cho Sniper?
```

**Kỳ vọng:** Bảng phân tích trade-off (ưu/nhược/rủi ro từng phương án), có khuyến nghị rõ ràng kèm lý do dựa trên dữ liệu wiki, không phải ý kiến chung chung không dẫn nguồn.

---

## Giai đoạn 3 — Đồng hành: Daily Companion, Reflection

### Test 8 — Luồng 8: Daily Companion

**Gõ (session mới):**
```
Chào buổi sáng
```

**Kỳ vọng:** Đủ 6 mục (raw/do, conflict, query log, wiki, partner-tracker, agreement) — đã pass ở lần test trước, giờ chỉ cần confirm vẫn ổn định qua nhiều lần.

---

### Test 9 — Luồng 9: Knowledge Interviewer

**Gõ:**
```
Reflection
```

**Kỳ vọng:** Kim hỏi 3-4 câu (không quá 4 + 1 follow-up), câu hỏi tự nhiên chứ không đọc y nguyên template. Trả lời thử vài câu ngắn gọn, xem Kim có tổng hợp đúng vào `_system/daily_reflection_log.md` theo đúng 6 trường (Bài học, Quyết định, Vấn đề đang mở...) không.

---

## Giai đoạn 4 — Dữ liệu sống: Partner Tracker, Agreement

### Test 10 — Luồng 11: Partner Tracker Sync

**Chuẩn bị:** Tạo file Excel `2026-07-01_doi-tac-test.xlsx` với các cột: `Ý kiến của tao`, `Ý kiến đối tác`, `Trạng thái`, `Loại vấn đề`, `Chức năng liên quan`. Điền 2-3 dòng test, ví dụ 1 dòng để trống "Ý kiến của tao" nhưng trạng thái ghi "Đang xử lý" (cố tình tạo dữ liệu mơ hồ để test Bước 4).

Copy vào `project-test-kim/partner-tracker/snapshots/`.

**Gõ:**
```
Sync partner tracker project-test-kim
```

**Kỳ vọng:**
- Nếu thư mục `partner-tracker/` chưa có, Kim tự tạo khi chạy lệnh này lần đầu
- Mỗi dòng được gán `ISSUE-001`, `ISSUE-002`...
- Dòng mơ hồ (thiếu ý kiến của tao) → **Kim phải hỏi lại**, không tự suy diễn — đây là điểm test quan trọng nhất của luồng này
- `tracker.md` được tạo đúng format (mục Đang mở / Đã đóng)

**Test tiếp (mô phỏng lần sync thứ 2):** Sửa trạng thái 1 dòng thành "Đã xử lý", lưu file mới `2026-07-05_doi-tac-test.xlsx`, chạy lại lệnh sync. Kỳ vọng: ISSUE đó **giữ nguyên ID cũ**, chỉ chuyển từ "Đang mở" xuống "Đã đóng".

---

### Test 11 — Luồng 12: Agreement Archive

**Chuẩn bị:** Lấy bất kỳ file `.docx` hoặc `.pdf` nào có sẵn, đổi tên thành `spec-test-v1-confirmed.docx`, copy vào `project-test-kim/agreements/active/`.

**Gõ:**
```
Archive agreement project-test-kim
```

**Kỳ vọng:**
- File giữ nguyên định dạng gốc, không bị convert
- `agreements/index.md` có 1 dòng mới: tên file, ngày, tóm tắt 1 câu **chỉ nói "về cái gì"** — thử đọc kỹ, nếu Kim diễn giải sâu nội dung điều khoản là **sai quy tắc tuyệt đối** (dòng 661 AGENTS_MASTER.md)

**Test tiếp (mô phỏng file thay thế):** Tạo thêm `spec-test-v2-confirmed.docx`, copy vào `agreements/active/`, gõ lại lệnh archive. Kỳ vọng: v1 tự động chuyển sang `archived/`, `index.md` cập nhật đúng 2 bảng (Active/Archived), có ghi lý do + file thay thế.

---

## Checklist tổng kết sau khi test xong 12 luồng

Đánh dấu lại luồng nào PASS / FAIL để biết chỗ nào cần quay lại debug với tao:

- [ ] Luồng 1 — Daily Scan (chờ xác nhận, không tự ingest)
- [ ] Luồng 2 — Ingest (metadata header đúng, wiki update đúng)
- [ ] Luồng 3 + Conflict — dừng đúng lúc, hỏi WHY bắt buộc
- [ ] Luồng 10 — cascade check chạy sau resolve
- [ ] Luồng 4 — Lint bắt đủ lỗi (broken link + mồ côi)
- [ ] Luồng 5 — Search phân biệt được nội dung cũ/mới
- [ ] Luồng 6 — Reasoning trình bày chuỗi suy luận, không chỉ trích dẫn
- [ ] Luồng 7 — Advisor có trade-off table + khuyến nghị có nguồn
- [ ] Luồng 8 — Daily Companion đủ 6 mục
- [ ] Luồng 9 — Reflection hỏi tự nhiên, tổng hợp đúng format
- [ ] Luồng 11 — Issue ID ổn định qua nhiều lần sync, hỏi lại khi mơ hồ
- [ ] Luồng 12 — Không diễn giải nội dung pháp lý, archive đúng khi có bản thay thế

Luồng nào FAIL, note lại nguyên văn câu trả lời sai của Kim rồi quay lại đây — tao debug theo đúng cách đã làm với Luồng 8.
