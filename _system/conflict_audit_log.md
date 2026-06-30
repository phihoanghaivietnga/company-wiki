# Conflict Audit Log

Nhật ký kiểm toán mâu thuẫn tri thức — ghi nhận mọi conflict được phát hiện trong toàn hệ thống wiki.
**Nguyên tắc bắt buộc**: Mọi quyết định giải quyết conflict phải ghi rõ **WHY** (lý do). Trường `lý_do` không được để trống.

---

## Cấu trúc một mục conflict

### Conflict đang pending

```markdown
### Conflict #{số} — {mô tả ngắn}
- **Ngày phát hiện**: YYYY-MM-DD
- **Trang 1**: [[ten-project/ten-trang-1]] — "{khẳng định A}"
- **Trang 2**: [[ten-project/ten-trang-2]] — "{khẳng định B}"
- **Nguồn gốc**: File nguồn liên quan (nếu có)
- **Mức độ**: Critical / Major / Minor
- **Đề xuất xử lý**: [phân tích ngắn về cách giải quyết]
- **Trạng thái**: `pending`
```

### Conflict đã resolved

```markdown
### Conflict #{số} — {mô tả ngắn}
- **Ngày phát hiện**: YYYY-MM-DD
- **Ngày giải quyết**: YYYY-MM-DD
- **Trang 1**: [[ten-project/ten-trang-1]] — "{khẳng định A}"
- **Trang 2**: [[ten-project/ten-trang-2]] — "{khẳng định B}"
- **Quyết định**: [chốt phương án nào — giữ A, giữ B, hoặc merge]
- **Lý do (WHY)**: [lý do bắt buộc — tại sao chọn phương án này]
- **Cập nhật wiki**: [trang nào đã sửa, thêm/xóa nội dung gì]
- **Cascade check**: [có phát sinh conflict mới không? Nếu có → tạo conflict mới]

--- **Trạng thái**: `resolved`
```

---

## Danh sách conflict

### Đang xử lý (Pending)

<!-- Thêm conflict mới vào đây -->

---

### Đã xử lý (Resolved)

<!-- Sau khi resolve, di chuyển conflict từ mục Pending xuống đây -->

---

## Ví dụ mẫu (template entry)

### Conflict #001 — Ví dụ: Mâu thuẫn về ngày release sản phẩm
- **Ngày phát hiện**: 2026-06-29
- **Trang 1**: [[project-sniper/release-schedule]] — "Sản phẩm X release vào Q3/2026"
- **Trang 2**: [[project-sniper/product-roadmap]] — "Sản phẩm X release vào Q4/2026"
- **Nguồn gốc**: file `spec-v2.md` vs `roadmap-2026.md`
- **Mức độ**: Major
- **Đề xuất xử lý**: Kiểm tra với stakeholder để xác nhận timeline chính xác. Nếu spec-v2 mới hơn roadmap → giữ Q3/2026, cập nhật roadmap.
- **Trạng thái**: `pending`

---

### Conflict #000 — Ví dụ: Mâu thuẫn về công nghệ backend (đã resolved)
- **Ngày phát hiện**: 2026-06-15
- **Ngày giải quyết**: 2026-06-16
- **Trang 1**: [[project-sniper/tech-stack]] — "Backend dùng Node.js + Express"
- **Trang 2**: [[project-sniper/adr-003]] — "ADR-003 chọn Python FastAPI cho backend"
- **Quyết định**: Giữ ADR-003 (Python FastAPI), cập nhật tech-stack.md
- **Lý do (WHY)**: ADR-003 được approve sau khi team benchmark cả 2 lựa chọn. FastAPI có performance tốt hơn cho use case xử lý ảnh, và team đã có kinh nghiệm Python. Quyết định đã được CTO phê duyệt.
- **Cập nhật wiki**: Sửa `tech-stack.md` — thay Node.js → Python FastAPI, thêm ref đến ADR-003
- **Cascade check**: Không phát sinh conflict mới.
- **Trạng thái**: `resolved`

---

## Quy tắc quản lý log

1. **Mọi conflict mới** được thêm vào mục "Đang xử lý (Pending)".
2. **Khi resolve xong**: di chuyển toàn bộ entry xuống mục "Đã xử lý (Resolved)", cập nhật đầy đủ các trường (ngày giải quyết, quyết định, lý do, cập nhật wiki, cascade check).
3. **Archive hàng tháng**: Nếu log quá dài (>20KB), tạo file archive `conflict_audit_log_YYYY-MM.md` và chỉ giữ lại các conflict đang pending + 30 ngày resolved gần nhất trong file chính.
4. **Trường `lý_do` không được để trống** — nếu người dùng chưa cung cấp lý do, AI phải hỏi: "Lý do bạn chọn phương án này là gì?"