# AGENTS.md — Project: test-kim

Kế thừa toàn bộ quy tắc từ `../AGENTS_MASTER.md` (Master Schema).
File này chỉ định nghĩa các thông tin và override **đặc thù của project này**.

> Khi có xung đột giữa file này và Master Schema → file này được ưu tiên.

---

## Thông tin project

- **Tên project**: test-kim
- **Mô tả ngắn**: Project sandbox để test 12 luồng của Kim
- **Ngày khởi tạo wiki**: 2026-07-01
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
project-test-kim/
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

---

## Override: Ưu tiên ingest

> Để trống nếu xử lý theo thứ tự file mặc định (alphabetical).

---

## Override: Knowledge Router — Phân loại tự động (Luồng 2 — Bước 0)

> Để trống nếu dùng cấu hình mặc định từ Master.

---

## Override: Contextual Search — Phạm vi tìm kiếm mặc định (Luồng 5)

> Để trống.

---

## Override: Strategic Advisor — Bối cảnh bổ sung (Luồng 7)

> Để trống.

---

## Override: Conflict Resolution — Quy tắc đặc thù (Luồng 10)

> Để trống — giữ nguyên tắc: AI không tự quyết.

---

## Liên kết đến _global/

Các khái niệm trong project này có thể tham chiếu đến `_global/wiki/` bằng cú pháp:
```
[[_global/ten-trang]]
```

---

## Ghi chú đặc thù khác

> Để trống.