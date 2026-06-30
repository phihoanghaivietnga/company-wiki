# AGENTS.md — Project: {TÊN PROJECT}

Kế thừa toàn bộ quy tắc từ `../AGENTS_MASTER.md` (Master Schema).
File này chỉ định nghĩa các thông tin và override **đặc thù của project này**.

> Khi có xung đột giữa file này và Master Schema → file này được ưu tiên.

---

## Thông tin project

- **Tên project**: WebApp
- **Mô tả ngắn**: Kho tri thức về ứng dụng webapp (tra cứu thông tin hồ sơ sức khoẻ và đặt lịch khám trên hệ thống CORE ISOFH)
- **Ngày khởi tạo wiki**: 2026-06-30
- **Trạng thái**: active 

---

## Phạm vi tri thức

Wiki của project này bao gồm:
- {Loại tài liệu 1 — ví dụ: spec sản phẩm, PRD, design doc}
- {Loại tài liệu 2 — ví dụ: quyết định kỹ thuật, ADR}
- {Loại tài liệu 3 — ví dụ: research, competitive analysis}

Wiki của project này **không bao gồm** (thuộc về `_global/` hoặc project khác):
- {Ví dụ: policy công ty, glossary chung}

---

## Cấu trúc thư mục project này

```text
project-{ten}/
├── AGENTS.md          ← File này
├── raw/
│   ├── do/            ← Nguồn mới chưa xử lý
│   └── done/          ← Nguồn đã xử lý (bất biến)
└── wiki/
    ├── index.md       ← Mục lục wiki của project
    ├── log.md         ← Nhật ký append-only
    └── thong_tin_mau_thuan.md  ← Conflict log
```

---

## Override: Ngôn ngữ wiki

> Để trống nếu giống Master (mặc định: tiếng Việt)

Ngôn ngữ wiki của project này: {Tiếng Việt / English / Mixed}

---

## Override: Cấu trúc trang đặc thù (nếu có)

> Để trống nếu dùng định dạng chuẩn từ Master Schema.

Nếu project này có loại trang đặc thù (ví dụ: ADR — Architecture Decision Record), định nghĩa template bổ sung ở đây:

```markdown
# [ADR-{số}] Tiêu đề quyết định

**Trạng thái**: Proposed | Accepted | Deprecated | Superseded

**Ngày**: YYYY-MM-DD

**Bối cảnh**: Mô tả vấn đề cần giải quyết.

**Quyết định**: Mô tả quyết định đã chọn.

**Hệ quả**: Những gì thay đổi sau quyết định này.

**Nguồn**: (nguồn: ten_file.md)
```

---

## Override: Ưu tiên ingest

> Để trống nếu xử lý theo thứ tự file mặc định (alphabetical).

Khi có nhiều file trong `raw/do/`, ưu tiên xử lý theo thứ tự:
1. {Loại file ưu tiên cao — ví dụ: file chứa "decision" trong tên}
2. {Loại file ưu tiên trung bình}
3. Còn lại xử lý theo thứ tự alphabetical

---

## Override: Knowledge Router — Phân loại tự động (Luồng 2 — Bước 0)

> Tùy chỉnh cách AI phân loại tài liệu khi ingest vào project này.
> Để trống nếu dùng cấu hình mặc định từ Master.

**Loại tri thức đặc thù của project này** (ngoài các loại mặc định: Quyết định kỹ thuật, Research, Spec, Policy):

| Loại tri thức | Mẫu tên file nguồn | Trang wiki bị ảnh hưởng điển hình | Ghi chú |
|---|---|---|---|
| {Ví dụ: UX Research} | {usertest-*.md} | {ux-findings.md, persona.md} | {Ghi chú} |
| {Ví dụ: Incident Report} | {incident-*.md} | {incident-log.md, postmortem.md} | {Ghi chú} |

**Quy tắc đặt tên trang tóm tắt** (nếu khác mặc định):
- Mặc định: đặt tên theo file nguồn (slug). Ví dụ: `spec-v2.md` → `spec-v2.md`
- Override của project này: {quy tắc riêng nếu có}

---

## Override: Contextual Search — Phạm vi tìm kiếm mặc định (Luồng 5)

> Tùy chỉnh phạm vi tìm kiếm khi người dùng gõ "Tìm thông tin về X".

**Project này có chứa loại thông tin đặc thù nào mà AI nên ưu tiên tìm không?**
- {Ví dụ: "Project này chứa toàn bộ quyết định về pricing — ưu tiên tìm ở đây trước"}
- {Hoặc để trống nếu không có gì đặc biệt}

**Project nào không nên tìm khi query về chủ đề cụ thể?**
- {Ví dụ: "Không tìm trong project-sniper khi hỏi về hardware — project đó chỉ về software"}
- {Hoặc để trống}

---

## Override: Strategic Advisor — Bối cảnh bổ sung (Luồng 7)

> Thông tin bổ sung AI nên cân nhắc khi tư vấn chiến lược cho project này.

**Ràng buộc đặc thù của project** (AI phải cân nhắc khi phân tích):
- {Ví dụ: "Budget tối đa $50K cho mọi quyết định mua công cụ"}
- {Ví dụ: "Không được chọn giải pháp yêu cầu server on-premise"}
- {Hoặc để trống}

**Stakeholder cần tham khảo** (nếu quyết định vượt quá phạm vi):
- {Ví dụ: "Quyết định về tech stack → cần CTO approve"}
- {Hoặc để trống}

---

## Override: Conflict Resolution — Quy tắc đặc thù (Luồng 10)

> Tùy chỉnh cách xử lý conflict cho project này.

**Ngưỡng conflict**: Mặc định flag tất cả conflict. Project này có loại conflict nào được bỏ qua không?
- {Ví dụ: "Minor formatting differences → bỏ qua, không flag"}
- {Hoặc để trống}

**Cascade check depth**: Mặc định kiểm tra cascade 2 levels. Project này có cần depth khác không?
- Mặc định: 2 levels
- Override: {số levels, hoặc để trống}

**Người duyệt conflict**: Mặc định là người dùng. Project này có quy tắc khác không?
- {Ví dụ: "Conflict loại Minor có thể tự resolve nếu overlap > 90%"}
- {Hoặc để trống — GIỮ NGUYÊN TẮC: AI không tự quyết}

---

## Liên kết đến _global/

Các khái niệm trong project này có thể tham chiếu đến `_global/wiki/` bằng cú pháp:
```
[[_global/ten-trang]]
```

Ví dụ: `[[_global/glossary]]`, `[[_global/company-policy]]`

---

## Ghi chú đặc thù khác

> Ghi bất kỳ quy tắc nào đặc thù của project mà không có trong Master Schema.

{Ví dụ: "Mọi trang kỹ thuật phải có section ## API Reference nếu liên quan đến API"}