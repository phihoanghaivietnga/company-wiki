# Hướng dẫn Vận hành Hàng ngày — Personal AI Knowledge Assistant

Tài liệu này hướng dẫn bạn vận hành hệ thống wiki + trợ lý AI hàng ngày.
**Không cần nhớ gì — chỉ cần đọc và làm theo.**

---

## 🗂️ Cấu trúc hệ thống (tóm tắt)

```text
company-wiki/
├── _global/raw/do/          ← 📥 THẢ FILE MỚI VÀO ĐÂY (hoặc project-X/raw/do/)
├── _global/wiki/            ← 📚 Tri thức đã tổ chức (AI ghi, bạn đọc)
├── project-{X}/raw/do/      ← 📥 File mới của project X
├── project-{X}/wiki/        ← 📚 Tri thức project X
├── project-{X}/partner-tracker/  ← 🔄 Trao đổi đối tác (Excel tracker, Luồng 11)
│   ├── snapshots/               ← File Excel gốc từng đợt (bất biến)
│   └── tracker.md               ← AI tổng hợp từng dòng issue
├── project-{X}/agreements/      ← 📜 Tài liệu đã thống nhất (Luồng 12)
│   ├── active/                  ← Còn hiệu lực
│   ├── archived/                ← Hết hiệu lực, giữ để đối chứng
│   └── index.md                 ← Mục lục agreement
├── _system/                 ← 📋 Trạng thái hệ thống (AI tự quản lý)
│   ├── daily_scan_report.md      ← Báo cáo scan hàng ngày
│   ├── conflict_audit_log.md     ← Mâu thuẫn cần giải quyết
│   ├── daily_reflection_log.md   ← Bài học hàng ngày
│   └── query_log.md              ← Câu hỏi hay chưa lưu
└── _templates/              ← 📝 Template (không cần chạm vào)
```

---

## 🔄 QUY TRÌNH HÀNG NGÀY (5 phút)

### 🌅 Buổi sáng: Khởi động ngày mới

**Bạn làm:**
1. Mở Cline chat
2. Gõ một trong các câu sau:
   - `"Chào buổi sáng"` — AI sẽ tóm tắt tình trạng, gợi ý việc cần làm
   - `"Hôm nay thế nào"` — tương tự

**AI sẽ làm:**
- Báo cáo: bao nhiêu file chưa xử lý, bao nhiêu conflict pending
- Gợi ý thứ tự ưu tiên công việc
- Nhắc bạn về các việc tồn đọng

**Thời gian**: ~1 phút

---

### 📥 Khi có tài liệu mới: Đưa vào hệ thống

**Bước 1 — Bạn thả file:**
- Copy file `.md`, `.pdf`, `.docx`, `.txt` vào `_global/raw/do/` (nếu là tri thức chung)
- Hoặc `project-X/raw/do/` (nếu là tri thức của project X)
- Nếu không chắc thuộc project nào → thả vào `_global/raw/do/`, AI sẽ phân loại giúp

**Bước 2 — Bạn gõ lệnh scan:**
- `"Scan toàn bộ wiki"` — quét tất cả project
- `"Scan _global"` — chỉ quét _global
- `"Scan project-sniper"` — chỉ quét 1 project

**Bước 3 — AI báo cáo:**
```
📋 KẾT QUẢ SCAN — 2026-06-30

| Project   | File mới | Tên file              |
|-----------|----------|-----------------------|
| _global   | 2        | policy.md, glossary.md|
| sniper    | 1        | feature-x.md          |

Bạn muốn ingest tất cả, hay chọn từng project?
```

**Bước 4 — Bạn xác nhận:**
- `"Ingest tất cả"` — AI xử lý lần lượt
- `"Ingest _global trước"` — chọn thứ tự

**Bước 5 — AI ingest:**
- AI đọc file, trích xuất tri thức, ghi vào wiki
- Nếu phát hiện mâu thuẫn với wiki hiện tại → AI dừng lại, hỏi bạn
- Sau khi xong → file được chuyển từ `raw/do/` sang `raw/done/`

**Bước 6 — AI quét conflict tổng:**
- Sau ingest, AI tự quét toàn wiki để tìm mâu thuẫn mới
- Ghi vào `_system/conflict_audit_log.md` (không làm phiền ngay)

**Thời gian**: 2-5 phút (tùy số lượng file)

---

### 🔄 KHI CÓ FILE EXCEL TRAO ĐỔI ĐỐI TÁC: Partner Tracker (Luồng 11)

Dùng khi bạn có file `.xlsx` đang trao đổi qua lại với đối tác (có cột trạng thái, ý kiến hai bên, còn cập nhật tiếp).

**Bước 1 — Bạn thả file:**
- Copy file `.xlsx` vào `project-X/partner-tracker/snapshots/`
- Đặt tên file theo format: `{YYYY-MM-DD}_{ten-file-goc}.xlsx` (ví dụ: `2026-06-29_partner-feedback.xlsx`)
- Nếu thư mục `partner-tracker/` chưa có → AI sẽ tự tạo khi bạn yêu cầu sync lần đầu

**Bước 2 — Bạn gõ lệnh sync:**
- `"Sync partner tracker project-X"` — AI sẽ đọc file Excel, diff với tracker.md, cập nhật issue

**AI sẽ làm:**
- Lưu snapshot file gốc (bất biến, không sửa)
- Đọc file Excel, xác định các cột: Ý kiến của user, Ý kiến đối tác, Trạng thái, Loại vấn đề
- So sánh với `tracker.md` hiện tại:
  - Issue mới → gán ID mới (`ISSUE-001`, `ISSUE-002`...)
  - Issue cũ thay đổi trạng thái → cập nhật, giữ nguyên ID
  - Issue đã đóng → chuyển xuống mục "Đã đóng"
- Nếu có dòng dữ liệu mơ hồ → AI sẽ hỏi bạn trước khi ghi
- Nếu issue liên quan đến trang wiki có sẵn → AI tự thêm link `[[trang]]`

**Bước 3 — Bạn xem kết quả:**
- Mở `project-X/partner-tracker/tracker.md` để xem tổng hợp
- Hoặc hỏi AI (xem mục bên dưới)

**Thời gian**: 1-2 phút / lần sync

---

### 🔍 CÁCH HỎI AI VỀ TÌNH TRẠNG ISSUE ĐANG MỞ VỚI ĐỐI TÁC

| Bạn muốn... | Gõ lệnh |
|---|---|
| Xem tất cả issue đang mở | `"Đối tác {tên} đang có những issue nào đang mở?"` |
| Xem chi tiết 1 issue | `"Chi tiết ISSUE-003 bên project-X?"` |
| Xem issue theo chức năng | `"Có issue nào liên quan đến paywall không?"` |
| Xem lịch sử 1 issue qua các lần sync | `"Lịch sử ISSUE-001 thay đổi thế nào?"` |

**AI sẽ:**
- Đọc `tracker.md` và trả lời dựa trên dữ liệu đã tổng hợp
- Nếu cần đối chiếu với spec trong wiki → AI sẽ tự tìm và dẫn link

---

### 📜 KHI CÓ FILE ĐÃ THỐNG NHẤT: Agreement Archive (Luồng 12)

Dùng khi bạn có file `.docx`, `.pdf`, hoặc `.xlsx` đã chốt, không thay đổi nữa — cần lưu làm bằng chứng đối chứng sau này.

**Quy tắc quan trọng:**
- AI **không được diễn giải** nội dung điều khoản, cam kết, số liệu trong file
- AI chỉ tóm tắt "về cái gì" và trỏ về bản gốc
- File gốc giữ nguyên định dạng, không convert

**Bước 1 — Bạn thả file:**
- Copy file gốc vào `project-X/agreements/active/`
- Giữ nguyên tên file gốc (ví dụ: `spec-v2-confirmed.docx`, `budget-q3-approved.xlsx`)

**Bước 2 — Bạn gõ lệnh:**
- `"Archive agreement project-X"` — AI sẽ quét thư mục `agreements/active/`, thêm file mới vào `index.md`
- Hoặc `"Thêm agreement {ten-file} vào project-X"` — thêm từng file

**AI sẽ làm:**
- Thêm 1 dòng vào bảng Active trong `agreements/index.md`: tên file, ngày, tóm tắt 1 câu (chỉ nói "về cái gì")
- Nếu file mới thay thế file cũ → AI tự chuyển file cũ sang `archived/`, cập nhật `index.md`
- Liên kết đến trang wiki liên quan nếu có

**Bước 3 — Khi file hết hiệu lực:**
- Gõ: `"Chuyển agreement {ten-file} sang archived"`
- AI sẽ move file + cập nhật `index.md` (ghi rõ lý do)

**Thời gian**: ~1 phút / file

---

### 🔍 CÁCH TRA CỨU AGREEMENT KHI CẦN ĐỐI CHỨNG

| Bạn muốn... | Gõ lệnh |
|---|---|
| Xem danh sách agreement đang hiệu lực | `"Project-X có những agreement nào đang active?"` |
| Tìm agreement theo từ khóa | `"Có agreement nào về ngân sách Q3 không?"` |
| Xem lịch sử thay thế 1 agreement | `"Agreement spec-v1 được thay thế bởi cái nào?"` |
| Mở file gốc để đối chứng | `"Mở agreement spec-v2-confirmed.docx"` hoặc tự mở từ `agreements/active/` |

**AI sẽ:**
- Đọc `agreements/index.md` và trả lời
- Luôn trỏ về file gốc, không diễn giải nội dung pháp lý

---

### ⚠️ Khi có conflict: Giải quyết mâu thuẫn

**Trigger**: AI báo conflict trong lúc ingest, hoặc bạn chủ động kiểm tra.

**Bạn gõ:**
- `"Xem conflict đang pending"` — AI liệt kê tất cả conflict chưa giải quyết
- Hoặc AI tự hỏi bạn khi ingest gặp conflict

**Format AI sẽ hiển thị:**
```
⚠️ PHÁT HIỆN CONFLICT — Cần xem xét

File mới   : B.md
Trang wiki : [[release-schedule.md]]
Nguồn cũ   : A.md

NỘI DUNG CŨ: Sản phẩm X release Q3/2026
NỘI DUNG MỚI: Sản phẩm X release Q4/2026

(a) Có — B thay thế hoàn toàn A
(b) Có một phần
(c) Không — cả hai đều đúng
(d) Không chắc
```

**Bạn trả lời:** `"a"` hoặc `"b"` hoặc `"c"` hoặc `"d"`

**AI hỏi tiếp:** `"Lý do bạn chọn phương án này là gì?"`

**Bạn trả lời lý do** (ví dụ: "Vì B là spec mới nhất đã được PO duyệt")

**AI tự động:**
- Cập nhật wiki
- Ghi WHY vào `_system/conflict_audit_log.md`
- Kiểm tra conflict cascade (có gây mâu thuẫn mới không?)
- Báo cáo kết quả: "Conflict #001 đã giải quyết. Không phát sinh conflict mới."

**Thời gian**: ~1 phút / conflict

---

### 🔍 Khi cần tìm thông tin: Tìm kiếm thông minh

Thay vì tự lục từng file, hãy hỏi AI:

| Bạn muốn... | Gõ lệnh |
|---|---|
| Tìm thông tin về X | `"Tìm tất cả thông tin về X"` |
| Tìm tài liệu liên quan Y | `"Tài liệu nào liên quan đến Y"` |
| Hỏi tại sao / phân tích | `"Tại sao Z...?"` hoặc `"Phân tích vấn đề W"` |
| Cần tư vấn chiến lược | `"Tư vấn về..."` hoặc `"Nên chọn A hay B"` |
| Hỏi kiến thức tổng hợp | `"Cho tôi biết về..."` |

**AI sẽ:**
- Đọc toàn bộ wiki liên quan (không chỉ keyword match)
- Nếu câu hỏi suy luận → xâu chuỗi thông tin, đưa ra kết luận có dẫn nguồn
- Nếu câu hỏi chiến lược → phân tích trade-off, đề xuất giải pháp (theo template `_templates/strategy-analysis.md`)
- Luôn hỏi: "Lưu câu trả lời này vào wiki không?"

**Thời gian**: 1-3 phút

---

### 🌙 Cuối ngày: Tổng kết & Reflection

**Bạn gõ:**
- `"Reflection"` hoặc `"Tổng kết hôm nay"`

**AI sẽ hỏi bạn 3-4 câu:**
- Kiểu: "Hôm nay bạn học được gì mới?", "Có quyết định nào quan trọng không?"
- Xoay vòng câu hỏi mỗi ngày (theo `_templates/reflection-prompt.md`)
- Không bao giờ hỏi quá 4 câu + 1 follow-up

**Bạn trả lời tự do**, không cần format.

**AI tổng hợp** vào `_system/daily_reflection_log.md`:
- Bài học mới
- Quyết định đã ra + lý do
- Vấn đề đang mở
- Đề xuất lưu insight vào wiki (nếu có)

**Thời gian**: 2-3 phút

---

## 🧹 BẢO TRÌ ĐỊNH KỲ (hàng tuần / hàng tháng)

### Hàng tuần (5 phút, thứ Sáu)

**Bạn gõ:** `"Lint toàn bộ"`

**AI kiểm tra:**
- Trang mồ côi (không ai link đến)
- Broken link `[[...]]`
- Nội dung thiếu nguồn
- Nội dung đánh dấu `[LỖI THỜI]` quá 30 ngày chưa xử lý
- Conflict chưa giải quyết

**AI báo cáo** + đề xuất fix.

---

### Hàng tháng (5 phút, đầu tháng)

**Bạn gõ:** `"Kiểm tra query log"`

**AI liệt kê:**
- Các Q&A có giá trị chưa lưu vào wiki
- Đề xuất: lưu hay bỏ qua?

**Bạn gõ:** `"Lưu tất cả"` hoặc chọn từng mục.

---

### Khi file `_system/` quá dài (AI tự nhắc)

Các file log sẽ được archive theo tháng:
- `conflict_audit_log_2026-06.md`
- `daily_reflection_log_2026-06.md`
- `daily_scan_report_2026-06.md`
- `query_log_2026-06.md`

**AI sẽ tự đề xuất** khi file chính >15-20KB.

---

## 📋 BẢNG TÓM TẮT LỆNH

| Lệnh | Chức năng | Khi nào dùng |
|---|---|---|
| `"Chào buổi sáng"` | Tóm tắt tình trạng + gợi ý | Mỗi sáng |
| `"Scan toàn bộ wiki"` | Quét file mới | Khi thả file vào raw/do/ |
| `"Ingest tất cả"` | Xử lý tất cả file mới | Sau khi scan |
| `"Ingest project-X"` | Chỉ xử lý 1 project | Khi cần ưu tiên |
| `"Tìm tất cả thông tin về X"` | Tìm kiếm ngữ cảnh | Khi cần tra cứu |
| `"Tại sao...?"` / `"Phân tích..."` | Suy luận từ wiki | Khi cần hiểu sâu |
| `"Tư vấn về X"` / `"Nên chọn A hay B"` | Phân tích chiến lược | Khi cần quyết định |
| `"Reflection"` | Phỏng vấn cuối ngày | Mỗi tối |
| `"Lint toàn bộ"` | Kiểm tra sức khỏe wiki | Hàng tuần |
| `"Xem conflict đang pending"` | Xem mâu thuẫn chưa giải quyết | Khi cần |
| `"Kiểm tra query log"` | Xem Q&A chưa lưu | Hàng tháng |
| `"Sync partner tracker project-X"` | Đồng bộ file Excel trao đổi đối tác | Khi có file .xlsx mới từ đối tác |
| `"Đối tác X đang có issue nào mở?"` | Hỏi tình trạng issue với đối tác | Khi cần nắm tình hình |
| `"Archive agreement project-X"` | Lưu file đã thống nhất vào agreement | Khi có file .docx/.pdf/.xlsx đã chốt |
| `"Project-X có agreement nào active?"` | Tra cứu agreement đang hiệu lực | Khi cần đối chứng |

---

## ⚡ QUY TRÌNH TÓM TẮT (in ra dán bàn làm việc)

```
🌅 SÁNG:    "Chào buổi sáng"
📥 CÓ FILE: Thả vào raw/do/ → "Scan toàn bộ wiki" → "Ingest tất cả"
🔄 PARTNER: Thả .xlsx vào partner-tracker/snapshots/ → "Sync partner tracker project-X"
📜 AGREEMENT: Thả file đã chốt vào agreements/active/ → "Archive agreement project-X"
⚠️ CONFLICT: Trả lời (a/b/c/d) + lý do khi AI hỏi
🔍 HỎI:    "Tìm thông tin về X" / "Tư vấn về Y" / "Tại sao Z?"
🔍 ĐỐI TÁC: "Đối tác X đang có issue nào mở?"
🔍 ĐỐI CHỨNG: "Project-X có agreement nào về Y?"
🌙 TỐI:    "Reflection"
📅 T6:      "Lint toàn bộ"
📅 ĐẦU THÁNG: "Kiểm tra query log"
```

---

## ❓ CÂU HỎI THƯỜNG GẶP

### Q: Tôi không chắc file mới thuộc project nào?
**A:** Thả vào `_global/raw/do/`. Khi ingest, AI sẽ phân loại và đề xuất project phù hợp (Wild #5 — Knowledge Router). Bạn chỉ cần xác nhận.

### Q: Conflict là gì? Tôi phải làm gì?
**A:** Conflict = hai nguồn thông tin nói hai điều khác nhau về cùng một vấn đề. AI sẽ hiển thị rõ ràng cả hai phiên bản và hỏi bạn chọn cái nào. Bạn chọn (a/b/c/d) + giải thích lý do. AI lo phần còn lại.

### Q: Tôi không muốn làm reflection hôm nay?
**A:** Không sao. Chỉ cần không gõ "Reflection". Không bắt buộc hàng ngày.

### Q: Tôi bận quá, không có thời gian scan?
**A:** File vẫn nằm trong `raw/do/`, không mất đi đâu. Khi nào rảnh thì scan. AI sẽ nhắc bạn nếu tồn đọng nhiều.

### Q: Làm sao biết wiki có bao nhiêu trang?
**A:** Vào `_global/wiki/index.md` hoặc `project-X/wiki/index.md` để xem. Đây là file mục lục do AI tự cập nhật.

### Q: AI có tự ý sửa wiki không?
**A:** Không. AI chỉ ghi wiki khi:
- Ingest file mới (sau khi bạn xác nhận)
- Giải quyết conflict (sau khi bạn chốt)
- Lint có thể đề xuất sửa nhưng luôn hỏi trước

### Q: Tôi muốn tìm project cũ đã archive?
**A:** Project archived vẫn có thư mục `project-X/`. AI vẫn tìm được khi bạn hỏi. Nếu không muốn AI tìm trong đó, thêm ghi chú vào `AGENTS.md` của project đó.

---

## 🛠️ THIẾT LẬP PROJECT MỚI

Khi có project mới cần thêm vào hệ thống:

**Bạn gõ:** `"Tạo project mới tên là X"`

**AI sẽ:**
1. Copy `AGENTS_PROJECT_TEMPLATE.md` → `project-X/AGENTS.md`
2. Tạo cấu trúc thư mục: `raw/do/`, `raw/done/`, `wiki/`
3. Tạo `wiki/index.md` trống
4. Tạo `wiki/log.md` với entry đầu tiên
5. Hỏi bạn điền thông tin project (mô tả, phạm vi tri thức)

**Sau đó**, bạn thả file vào `project-X/raw/do/` và scan như bình thường.

---

## 📊 KIỂM TRA SỨC KHỎE HỆ THỐNG

Bất kỳ lúc nào, gõ: `"Báo cáo sức khỏe hệ thống"`

AI sẽ tổng hợp:
- Số project, số trang wiki
- File pending trong `raw/do/`
- Conflict pending
- Query log chưa lưu
- Lần lint cuối cùng
- Cảnh báo nếu có vấn đề

---

*Hướng dẫn này được tạo ngày 2026-06-30. Cập nhật khi có thay đổi quy trình.*