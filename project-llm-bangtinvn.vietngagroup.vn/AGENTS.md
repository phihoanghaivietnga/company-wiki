# AGENTS.md — Project: Bảng Tin VN (bangtinvn.vietngagroup.vn)

Kế thừa toàn bộ quy tắc từ `../AGENTS_MASTER.md` (Master Schema).
File này chỉ định nghĩa các thông tin và override **đặc thù của project này**.

> Khi có xung đột giữa file này và Master Schema → file này được ưu tiên.

---

## Thông tin project

- **Tên project**: Bảng Tin VN (bangtinvn.vietngagroup.vn)
- **Mô tả ngắn**: Hệ thống bảng tin nội bộ — VietNga Group
- **Ngày khởi tạo wiki**: 2026-06-30
- **Trạng thái**: active

---

## Phạm vi tri thức

Wiki của project này bao gồm:
- Spec sản phẩm, PRD, design doc
- Quyết định kỹ thuật, kiến trúc hệ thống
- Research, tài liệu người dùng

Wiki của project này **không bao gồm** (thuộc về `_global/` hoặc project khác):
- Policy công ty, glossary chung

---

## Cấu trúc thư mục project này

```text
project-llm-bangtinvn.vietngagroup.vn/
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

## Liên kết đến _global/

Các khái niệm trong project này có thể tham chiếu đến `_global/wiki/` bằng cú pháp:
```
[[_global/ten-trang]]
```

Ví dụ: `[[_global/glossary]]`, `[[_global/company-policy]]`

---

## Ghi chú đặc thù khác

- Dự án bảng tin nội bộ, phục vụ nhân viên trong tập đoàn