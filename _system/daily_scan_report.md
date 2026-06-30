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

## 2026-06-30 — Daily Scan

### Tổng quan
- **Tổng file mới trong raw/do/**: 1
- **Đã ingest**: 1
- **Conflict phát hiện**: 0
- **Conflict đã resolve**: 0
- **Wiki pages**: ~52 (tạo mới: 1, cập nhật: 0)
- **Lỗi**: Không có

### Chi tiết theo project

| Project | File mới | Đã ingest | Conflict mới | Conflict resolved | Trang wiki ảnh hưởng |
|---|---|---|---|---|---|
| bangtinvn | 1 | 1 | 0 | 0 | fix-babel-import-error.md |
| _global | 0 | 0 | 0 | 0 | — |
| db-hisnano_v2 | 0 | 0 | 0 | 0 | — |
| bod | 0 | 0 | 0 | 0 | — |
| core-isofh | 0 | 0 | 0 | 0 | — |
| etl_v2 | 0 | 0 | 0 | 0 | — |
| webapp | 0 | 0 | 0 | 0 | — |

### Conflict cần chú ý

Không có conflict pending.

### 🔍 Lint toàn bộ (09:41 — 30/06/2026)

| Project | Wiki pages | Broken link | Mồ côi | Thiếu nguồn | [LỖI THỜI] | Đánh giá |
|---|---|---|---|---|---|---|
| bangtinvn | 1 | 1 (đã fix) | 0 | 0 | 0 | ✅ Sạch |
| _global | 0 | 0 | 0 | — | 0 | ✅ Skeleton, chưa có trang |
| db-hisnano_v2 | 0 | 0 | 0 | — | 0 | ✅ Skeleton, chưa có trang |
| bod | 10 | 0 | 0 | 0 | 0 | ✅ Sạch |
| core-isofh | 18 | 0 | 0 | 0 | 0 | ✅ Sạch |
| etl_v2 | 11 | 0 | 0 | 0 | 0 | ✅ Sạch |
| webapp | 14 | 0 | 0 | 0 | 0 | ✅ Sạch |

#### Chi tiết sửa chữa
- **bangtinvn/fix-babel-import-error.md**: Broken link `[[tong-quan-he-thong]]` → thay bằng comment placeholder. Link này trỏ tới trang thuộc project webapp, không tồn tại trong phạm vi bangtinvn.

### Hành động khuyến nghị
1. ~~Lint toàn bộ 6 project~~ ✅ Đã hoàn thành — 1 broken link đã fix
2. Reflection cuối ngày — capture bài học từ đợt nâng cấp lớn

### Ghi chú
- Hệ thống vừa được rà soát và chuẩn hóa cấu trúc thư mục: tạo mới `_global/`, bổ sung file nền tảng cho bangtinvn và db-hisnano_v2, thêm `thong_tin_mau_thuan.md` cho 4 project thiếu
- Lint toàn bộ 7 project (gồm _global): chỉ phát hiện 1 broken link duy nhất, đã fix. Tất cả project đều sạch — không có trang mồ côi, không thiếu nguồn, không có nội dung [LỖI THỜI].

---

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