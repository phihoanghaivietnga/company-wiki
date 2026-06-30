# CONTEXT.md — GSD Session: Personal AI Knowledge Assistant

## Nhiệm vụ của session này
Nâng cấp hệ thống LLM-Wiki hiện tại thành Personal AI Knowledge Assistant toàn diện bằng cách:
1. Cập nhật `AGENTS_MASTER.md` với các luồng mới (Phase 1 + Phase 2)
2. Cập nhật `AGENTS_PROJECT_TEMPLATE.md` với các override mới
3. Tạo tất cả file `_system/` và `_templates/` theo đúng format trong PRD
4. Tạo tài liệu vận hành hàng ngày `OPERATIONS_GUIDE.md`

**Không làm trong session này**: Wild #3 (DB connector), Wild #8 (calendar integration) — Phase 3.

---

## Môi trường thực thi

- **Agent**: Cline + Qwen3 (OpenRouter)
- **Người dùng**: 1 người, duy nhất
- **OS**: Local (không cần chạy code, chỉ tạo/chỉnh sửa file markdown)
- **Thư mục gốc dự án**: `company-wiki/` (agent tự tạo nếu chưa có)
- **Ngôn ngữ nội dung**: Tiếng Việt

---

## File đầu vào (đọc trước khi làm bất cứ điều gì)

| File | Vai trò |
|---|---|
| `PRD_Personal_AI_Assistant.md` | Yêu cầu chức năng đầy đủ — nguồn sự thật duy nhất |
| `AGENTS_MASTER.md` | File hiện tại cần cập nhật — không xóa, chỉ mở rộng |
| `AGENTS_PROJECT_TEMPLATE.md` | File hiện tại cần cập nhật |

---

## Deliverables bắt buộc (checklist hoàn thành)

### A. File cập nhật
- [ ] `AGENTS_MASTER.md` — thêm Luồng 5–10, rules mới
- [ ] `AGENTS_PROJECT_TEMPLATE.md` — thêm override section cho các luồng mới

### B. File mới trong `_system/`
- [ ] `_system/conflict_audit_log.md` — format chuẩn, có example entry
- [ ] `_system/daily_reflection_log.md` — format chuẩn, có example entry
- [ ] `_system/query_log.md` — format chuẩn, có example entry
- [ ] `_system/daily_scan_report.md` — format chuẩn (nếu chưa tồn tại)

### C. File mới trong `_templates/`
- [ ] `_templates/reflection-prompt.md` — 5 câu hỏi rotation
- [ ] `_templates/strategy-analysis.md` — template phân tích chiến lược

### D. Tài liệu vận hành
- [ ] `OPERATIONS_GUIDE.md` — hướng dẫn vận hành hàng ngày đầy đủ

---

## Ràng buộc kỹ thuật

- Mọi file markdown phải self-contained — agent đọc 1 file là đủ để thực thi
- Không dùng placeholder mơ hồ như "thêm nội dung ở đây" — phải có example cụ thể
- Format nhất quán: header YAML cho file log, table cho index, code block cho template
- `AGENTS_MASTER.md` phải là file duy nhất một agent mới cần đọc để hiểu toàn bộ hệ thống

---

## Thứ tự thực hiện (bắt buộc theo đúng thứ tự)

1. Đọc toàn bộ 3 file đầu vào
2. Tạo `_system/conflict_audit_log.md`
3. Tạo `_system/daily_reflection_log.md`
4. Tạo `_system/query_log.md`
5. Tạo `_system/daily_scan_report.md`
6. Tạo `_templates/reflection-prompt.md`
7. Tạo `_templates/strategy-analysis.md`
8. Cập nhật `AGENTS_PROJECT_TEMPLATE.md`
9. Cập nhật `AGENTS_MASTER.md` (làm cuối vì cần tham chiếu tất cả file trên)
10. Tạo `OPERATIONS_GUIDE.md`
11. Báo cáo checklist hoàn thành — tick từng mục trong Deliverables

---

## Định nghĩa "Hoàn thành"

Session này hoàn thành khi và chỉ khi:
- Tất cả 11 mục trong checklist Deliverables đã được tick
- `AGENTS_MASTER.md` có thể đứng độc lập — một agent mới đọc file này là đủ để vận hành toàn bộ hệ thống mà không cần hỏi thêm
- `OPERATIONS_GUIDE.md` đủ chi tiết để người dùng thực hiện quy trình hàng ngày không cần nhớ gì thêm
