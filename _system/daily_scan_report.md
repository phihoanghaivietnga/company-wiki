# Daily Scan Report

Báo cáo tổng hợp sau mỗi lần quét toàn hệ thống (Daily Scan — Luồng 1).
Ghi nhận trạng thái ingest, conflict phát hiện, và các hành động đã thực hiện trong ngày.

---

## Cấu trúc một mục báo cáo

```markdown
## YYYY-MM-DD — Daily Scan

### Tổng quan
- **Tổng file mới trong raw/do/**: {số lượng}
- **Đã ingest**: {số lượng}
- **Conflict phát hiện**: {số lượng}
- **Conflict đã resolve**: {số lượng}
- **Wiki pages**: {tổng số trang} (tạo mới: {số}, cập nhật: {số})
- **Lỗi**: {số lượng / mô tả}

### Chi tiết theo project

| Project | File mới | Đã ingest | Conflict mới | Conflict resolved | Trang wiki ảnh hưởng |
|---|---|---|---|---|---|
| _global | 2 | 2 | 0 | 0 | policy.md, glossary.md |
| project-sniper | 1 | 1 | 1 | 0 | paywall-ux.md |
| project-X | 0 | 0 | 0 | 1 | — |

### Conflict cần chú ý

Các conflict đang pending trong `_system/conflict_audit_log.md`:
- **Conflict #001** (project-sniper) — Mâu thuẫn ngày release — pending từ 2026-06-29
- **Conflict #002** (project-X) — [mô tả ngắn] — pending từ YYYY-MM-DD

### Hành động khuyến nghị
1. [Hành động 1 — ví dụ: Resolve conflict #001]
2. [Hành động 2 — ví dụ: Review query log có 3 Q&A chưa lưu]
3. [Hành động 3 — ví dụ: Lint project-Y]

### Ghi chú
- [Ghi chú đặc biệt nếu có]
```

---

## Danh sách báo cáo

<!-- AI tự động thêm báo cáo mới vào đầu file sau mỗi lần Daily Scan -->

---

## Ví dụ mẫu

## 2026-06-29 — Daily Scan

### Tổng quan
- **Tổng file mới trong raw/do/**: 3
- **Đã ingest**: 3
- **Conflict phát hiện**: 1
- **Conflict đã resolve**: 0
- **Wiki pages**: 47 (tạo mới: 2, cập nhật: 5)
- **Lỗi**: Không có

### Chi tiết theo project

| Project | File mới | Đã ingest | Conflict mới | Conflict resolved | Trang wiki ảnh hưởng |
|---|---|---|---|---|---|
| _global | 2 | 2 | 0 | 0 | policy-v2.md, glossary.md |
| project-sniper | 1 | 1 | 1 | 0 | paywall-ux.md |
| project-gamma | 0 | 0 | 0 | 0 | — |

### Conflict cần chú ý

Các conflict đang pending trong `_system/conflict_audit_log.md`:
- **Conflict #001** (project-sniper) — Mâu thuẫn ngày release sản phẩm X: Q3 vs Q4/2026 — pending từ 2026-06-29

### Hành động khuyến nghị
1. Resolve conflict #001 về ngày release — kiểm tra với stakeholder
2. Query log có 2 Q&A chưa lưu vào wiki — review và quyết định

### Ghi chú
- Hệ thống mới nâng cấp lên Personal AI Knowledge Assistant — các luồng Wild #1-#11 đã được tích hợp vào AGENTS_MASTER.md

---

## Quy tắc quản lý

1. **Mỗi ngày 1 báo cáo** — AI ghi vào đầu file sau khi hoàn tất Daily Scan.
2. **Nếu không có gì để scan** (raw/do/ trống) → vẫn ghi báo cáo với tổng quan "Không có file mới — hệ thống sạch".
3. **Archive theo tháng**: Nếu file >15KB, tạo `daily_scan_report_YYYY-MM.md` và bắt đầu file mới.
4. **Tham chiếu chéo**: Luôn liên kết đến `_system/conflict_audit_log.md` khi đề cập đến conflict.