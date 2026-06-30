# PRD: Personal AI Knowledge Assistant
## Nâng cấp LLM-Wiki → Hệ thống Trợ lý AI Toàn diện

**Phiên bản**: 1.0  
**Ngày**: 2026-06-29  
**Trạng thái**: Ready for GSD Planning

---

## 1. Bối cảnh & Mục tiêu

### 1.1 Hiện trạng
Hệ thống LLM-Wiki hiện tại (Karpathy model, đã được mở rộng) xử lý tốt:
- Ingest tài liệu nguồn → wiki có cấu trúc
- Detect và flag conflict
- Lint / audit wiki

### 1.2 Khoảng trống
Hệ thống hiện tại vẫn **bị động** — chỉ xử lý khi người dùng gõ lệnh.
11 Wild Ideas mô tả một hệ thống **chủ động**, có tư duy, có vòng lặp tự cải thiện.

### 1.3 Mục tiêu sau nâng cấp
Biến AGENTS.md thành blueprint cho một trợ lý AI cá nhân có khả năng:
1. **Tìm kiếm ngữ cảnh** thay vì keyword search
2. **Suy luận** từ dữ liệu, không chỉ trích xuất
3. **Tư vấn chiến lược** như senior colleague
4. **Tự tổ chức** và bảo trì tri thức
5. **Chủ động khai thác** tri thức từ người dùng
6. **Đóng vòng lặp** conflict resolution hoàn chỉnh

### 1.4 Ngoài phạm vi (Phase 2)
- Wild #3: Kết nối cơ sở dữ liệu thực tế (cần DB connector riêng)
- Wild #8: Điều phối lịch/task tự động (cần calendar integration)

---

## 2. Kiến trúc hệ thống mới

### 2.1 Cấu trúc thư mục bổ sung

```text
company-wiki/
├── AGENTS.md                          ← Schema master (đã có, sẽ mở rộng)
├── _global/
│   ├── raw/do/
│   ├── raw/done/
│   └── wiki/
├── project-{ten}/
│   ├── AGENTS.md
│   ├── raw/do/
│   ├── raw/done/
│   └── wiki/
├── _system/
│   ├── daily_scan_report.md           ← Đã có
│   ├── conflict_audit_log.md          ← MỚI: Wild #11 — nhật ký kiểm toán
│   ├── daily_reflection_log.md        ← MỚI: Wild #9 — nhật ký tri thức hàng ngày
│   └── query_log.md                   ← MỚI: log các câu hỏi có giá trị để lưu vào wiki
└── _templates/
    ├── reflection-prompt.md           ← MỚI: template phỏng vấn hàng ngày
    └── strategy-analysis.md           ← MỚI: template phân tích chiến lược
```

### 2.2 Các loại tri thức mới cần quản lý

| Loại | Nguồn | Lưu ở đâu | Trigger |
|---|---|---|---|
| Tài liệu tĩnh | File trong raw/do/ | wiki/ bình thường | Ingest command |
| Tri thức từ hội thoại | Câu hỏi + trả lời có giá trị | wiki/ (đề xuất lưu) | Sau mỗi Q&A |
| Bài học hàng ngày | Phỏng vấn cuối ngày | _system/daily_reflection_log.md | Daily reflection |
| Conflict chưa xử lý | Phát hiện khi ingest | _system/conflict_audit_log.md | Tự động khi có conflict |
| Quyết định + lý do | Sau khi user chốt conflict | wiki/thong_tin_mau_thuan.md | Sau conflict resolution |

---

## 3. Yêu cầu chức năng chi tiết

### 3.1 Wild #1 — Lục lọi đa chiều (Contextual Search)

**Mô tả**: Tìm kiếm theo ngữ cảnh, không chỉ keyword.

**Trigger**: `"Tìm tất cả thông tin về X"` hoặc `"Tài liệu nào liên quan đến Y"`

**Quy trình**:
1. Đọc `wiki/index.md` của tất cả project liên quan
2. Xác định các trang có khả năng chứa thông tin (không chỉ exact match tên)
3. Đọc nội dung các trang đó
4. Tổng hợp kết quả theo format:

```markdown
## Kết quả tìm kiếm: "{query}"

### Tìm thấy trong wiki
- **[[ten-trang]]** (project-X) — [lý do liên quan]
- **[[ten-trang-2]]** (_global) — [lý do liên quan]

### Tìm thấy trong raw/done/
- **A.md** (project-Y, ingest: YYYY-MM-DD) — [đoạn trích liên quan]

### Không tìm thấy trong
- project-Z — Không có thông tin về chủ đề này
```

**Lưu ý**: Nếu tìm thấy câu trả lời có giá trị → đề xuất lưu vào wiki.

---

### 3.2 Wild #2 — Suy luận từ dữ liệu cục bộ (Local Reasoning)

**Mô tả**: Xâu chuỗi thông tin từ nhiều trang wiki để suy luận ra kết luận mới.

**Trigger**: Câu hỏi dạng "Tại sao...", "Điều gì sẽ xảy ra nếu...", "Nên làm gì khi..."

**Quy trình**:
1. Xác định đây là câu hỏi suy luận (không có câu trả lời trực tiếp trong wiki)
2. Thu thập tất cả trang wiki liên quan
3. Xâu chuỗi thông tin, suy luận ra kết luận
4. Trình bày rõ **chuỗi suy luận**:

```markdown
## Phân tích: "{câu hỏi}"

### Dữ liệu thu thập
- Từ [[trang-A]]: [thông tin 1]
- Từ [[trang-B]]: [thông tin 2]

### Chuỗi suy luận
[Bước 1] → [Bước 2] → [Kết luận]

### Kết luận
[Câu trả lời cuối cùng]

### Độ tin cậy
- Cao / Trung bình / Thấp — [lý do]

> Đề xuất lưu kết luận này vào wiki không?
```

---

### 3.3 Wild #4 — Chuyên gia tư vấn chiến lược (Strategic Advisor)

**Mô tả**: Phân tích vấn đề từ nhiều góc nhìn, đề xuất giải pháp tối ưu.

**Trigger**: `"Tư vấn về X"`, `"Nên chọn A hay B"`, `"Phân tích vấn đề Y"`

**Quy trình**:
1. Đọc toàn bộ wiki liên quan đến vấn đề
2. Xác định các góc nhìn khác nhau từ dữ liệu
3. Phân tích trade-off
4. Đề xuất giải pháp với lý luận rõ ràng

**Template output** (`_templates/strategy-analysis.md`):
```markdown
## Phân tích chiến lược: "{vấn đề}"

### Bối cảnh (từ wiki)
[Tóm tắt thông tin liên quan từ wiki]

### Các góc nhìn
| Góc nhìn | Ưu | Nhược | Nguồn |
|---|---|---|---|
| Phương án A | ... | ... | [[trang]] |
| Phương án B | ... | ... | [[trang]] |

### Khuyến nghị
**Chọn [X] vì**: [lý do dựa trên dữ liệu]

### Rủi ro cần lưu ý
- [Rủi ro 1]
- [Rủi ro 2]

> Lưu phân tích này vào wiki?
```

---

### 3.4 Wild #5 — Thủ thư định tuyến tri thức (Knowledge Router)

**Mô tả**: Khi ingest file mới, tự động xác định chính xác nó thuộc project nào, loại tri thức nào, và link đến các trang liên quan.

**Tích hợp vào Luồng 2 (Ingest)** — bổ sung Bước 0:

**Bước 0: Phân loại tự động trước khi ingest**
```markdown
## Phân loại tài liệu: "{tên file}"

- **Thuộc project**: project-X (dựa trên nội dung)
- **Loại tri thức**: [Quyết định kỹ thuật / Research / Spec / Policy / Khác]
- **Trang wiki sẽ bị ảnh hưởng**: [[trang-A]], [[trang-B]]
- **Đề xuất đặt tên trang tóm tắt**: `ten-file-slug.md`

Xác nhận để tiếp tục?
```

Nếu file nằm sai thư mục project → cảnh báo và đề xuất di chuyển.

---

### 3.5 Wild #6 — Máy dò mâu thuẫn logic (Conflict Detector)

**Mô tả**: Phiên bản nâng cao của conflict detection hiện tại — không chỉ detect khi ingest mà còn **quét chủ động** toàn wiki.

**Trigger tự động**: Chạy trong Daily Scan (sau bước ingest).

**Quy trình quét chủ động**:
1. Đọc tất cả trang wiki trong một project
2. Tìm các khẳng định mâu thuẫn giữa các trang khác nhau
3. Flag vào `_system/conflict_audit_log.md` (không làm phiền ngay)

**Format `_system/conflict_audit_log.md`**:
```markdown
# Conflict Audit Log

## [YYYY-MM-DD] — Chưa xử lý

### Conflict #001
- **Trang 1**: [[trang-A]] — "{khẳng định A}"
- **Trang 2**: [[trang-B]] — "{khẳng định B}"
- **Mức độ**: Critical / Major / Minor
- **Đề xuất xử lý**: [phân tích ngắn]
- **Trạng thái**: `pending`

---

## [YYYY-MM-DD] — Đã xử lý

### Conflict #000
- **Quyết định**: [chốt phương án nào]
- **Lý do**: [why — bắt buộc ghi]
- **Cập nhật wiki**: [trang nào đã sửa]
- **Trạng thái**: `resolved`
```

---

### 3.6 Wild #7 — Người bạn đồng hành thường nhật (Daily Companion)

**Mô tả**: Chế độ hội thoại nhẹ — AI hỏi thăm, tóm tắt ngày làm việc, gợi ý.

**Trigger**: `"Chat"`, `"Hôm nay thế nào"`, hoặc bắt đầu ngày làm việc.

**Quy trình**:
1. Đọc `_system/daily_scan_report.md` (ngày hôm nay)
2. Đọc `_system/conflict_audit_log.md` (pending items)
3. Tóm tắt tình trạng hệ thống wiki
4. Gợi ý việc cần làm hôm nay dựa trên pending items

**Output mẫu**:
```
Chào buổi sáng! Tóm tắt nhanh:

📥 raw/do/: 3 file chưa xử lý (project-sniper: 2, _global: 1)
⚠️  Conflict pending: 2 (từ hôm qua, cần review)
✅ Wiki: 47 trang, cập nhật lần cuối hôm qua

Hôm nay nên ưu tiên:
1. Resolve 2 conflict trong conflict_audit_log.md
2. Ingest 3 file mới (ước tính ~15 phút)
```

---

### 3.7 Wild #9 — Phóng viên khai phá tri thức (Knowledge Interviewer)

**Mô tả**: AI chủ động phỏng vấn người dùng cuối ngày, khai thác bài học và lưu vào wiki.

**Trigger**: `"Reflection"`, `"Tổng kết hôm nay"`, hoặc theo schedule cuối ngày.

**Quy trình**:
1. AI đặt 3–5 câu hỏi theo template (`_templates/reflection-prompt.md`)
2. Người dùng trả lời tự do
3. AI tổng hợp thành tri thức có cấu trúc
4. Đề xuất lưu vào wiki page phù hợp hoặc tạo trang mới

**Template `_templates/reflection-prompt.md`**:
```markdown
## Câu hỏi reflection hàng ngày

1. Hôm nay bạn học được điều gì mới nhất?
2. Có quyết định nào bạn đưa ra — lý do là gì?
3. Có vấn đề nào chưa giải quyết được — bạn đã thử cách nào?
4. Điều gì hoạt động tốt hơn bạn mong đợi?
5. Nếu làm lại hôm nay, bạn sẽ thay đổi gì?
```

**Format lưu vào `_system/daily_reflection_log.md`**:
```markdown
## [YYYY-MM-DD]

**Bài học**: [tóm tắt từ câu trả lời]
**Quyết định đã ra**: [quyết định + lý do]
**Vấn đề đang mở**: [chưa giải quyết]
**Tri thức mới → wiki**: [[trang-được-tạo/cập-nhật]]
```

---

### 3.8 Wild #10 — Vòng lặp phản hồi mâu thuẫn (Conflict Feedback Loop)

**Mô tả**: Sau khi người dùng chốt conflict, AI tự động:
1. Cập nhật wiki
2. Kiểm tra lại xem conflict đã thực sự biến mất chưa
3. Tìm các conflict cascade (conflict A giải quyết xong → có gây ra conflict B không?)

**Bổ sung vào Luồng Xử lý Conflict** — sau khi người dùng chọn phương án:

```
Bước sau khi user chốt:
1. Cập nhật wiki page liên quan
2. Cập nhật conflict_audit_log.md: status → resolved + ghi lý do (WHY)
3. Quét lại các trang liên quan để check cascade conflict
4. Báo cáo: "Conflict #001 đã giải quyết. Không phát sinh conflict mới."
   hoặc: "Conflict #001 đã giải quyết. Phát hiện conflict mới #002 liên quan."
```

---

### 3.9 Wild #11 — Nhật ký kiểm toán tri thức (Knowledge Audit Log)

**Mô tả**: Tích hợp vào Daily Scan — quét ngầm, gom conflict vào log, không làm phiền ngay.

**Đã được tích hợp vào Wild #6** (conflict_audit_log.md).

**Điểm bổ sung**: Bảo tồn **WHY** — mọi quyết định giải quyết conflict phải ghi lý do, không chỉ kết quả.

**Rule bắt buộc trong AGENTS.md**:
> Khi người dùng chốt phương án giải quyết conflict, AI phải hỏi: "Lý do bạn chọn phương án này là gì?" trước khi ghi vào log. Trường `lý_do` không được để trống.

---

## 4. Cập nhật AGENTS.md Master

### 4.1 Các luồng mới cần thêm vào AGENTS.md

| Luồng | Tên | Trigger |
|---|---|---|
| Luồng 5 | Contextual Search | `"Tìm thông tin về X"` |
| Luồng 6 | Local Reasoning | Câu hỏi suy luận mở |
| Luồng 7 | Strategic Advisor | `"Tư vấn..."`, `"Nên chọn..."` |
| Luồng 8 | Daily Companion | `"Chat"`, buổi sáng |
| Luồng 9 | Knowledge Interview | `"Reflection"`, cuối ngày |
| Luồng 10 | Conflict Feedback Loop | Tự động sau conflict resolution |

### 4.2 Files mới cần tạo trong `_system/`

| File | Mô tả | Ai ghi |
|---|---|---|
| `conflict_audit_log.md` | Nhật ký conflict — pending + resolved + WHY | AI tự động |
| `daily_reflection_log.md` | Bài học hàng ngày từ phỏng vấn | AI sau reflection session |
| `query_log.md` | Câu hỏi + trả lời có giá trị, chưa lưu vào wiki | AI đề xuất sau Q&A |

### 4.3 Files mới cần tạo trong `_templates/`

| File | Dùng cho |
|---|---|
| `reflection-prompt.md` | Wild #9 — câu hỏi phỏng vấn cuối ngày |
| `strategy-analysis.md` | Wild #4 — template phân tích chiến lược |

---

## 5. Phân chia Phase thực hiện

### Phase 1 (Core — làm ngay)
Mở rộng AGENTS.md Master với:
- [ ] Luồng 5: Contextual Search
- [ ] Luồng 6: Local Reasoning  
- [ ] Luồng 8: Daily Companion
- [ ] Luồng 10: Conflict Feedback Loop (nâng cấp từ conflict handling hiện tại)
- [ ] Wild #11: conflict_audit_log.md + bắt buộc ghi WHY
- [ ] Wild #5: Knowledge Router (tích hợp vào Luồng 2 Ingest)

### Phase 2 (Advanced)
- [ ] Luồng 7: Strategic Advisor + template
- [ ] Luồng 9: Knowledge Interviewer + reflection template
- [ ] Wild #6: Active conflict scan (quét toàn wiki chủ động)

### Phase 3 (Tooling — cần tích hợp ngoài)
- [ ] Wild #3: DB connector (cần thiết kế riêng)
- [ ] Wild #8: Calendar/task integration

---

## 6. Tiêu chí hoàn thành (Definition of Done)

- [ ] AGENTS_MASTER.md được cập nhật với tất cả luồng Phase 1
- [ ] AGENTS_PROJECT_TEMPLATE.md có placeholder cho các override mới
- [ ] Tất cả file `_system/` và `_templates/` được tạo với format chuẩn
- [ ] Cline/Qwen3 có thể thực thi đúng tất cả luồng mà không cần hướng dẫn thêm
- [ ] Conflict luôn được flag + bắt buộc ghi WHY trước khi resolve

---

## 7. Rủi ro và giải pháp

| Rủi ro | Xác suất | Giải pháp |
|---|---|---|
| Qwen3 không đủ context window để quét toàn wiki | Trung bình | Giới hạn scope mỗi lần query; ưu tiên index.md trước |
| conflict_audit_log.md phình to theo thời gian | Cao | Archive log theo tháng: `conflict_audit_log_2026-06.md` |
| Reflection session trở nên nhàm chán, bỏ qua | Trung bình | Rotate câu hỏi, không hỏi tất cả 5 câu mỗi ngày |
| Cascade conflict tạo vòng lặp vô hạn | Thấp | Giới hạn depth cascade check = 2 levels |
