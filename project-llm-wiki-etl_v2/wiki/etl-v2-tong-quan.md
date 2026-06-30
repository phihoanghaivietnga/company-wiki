# ETL V2 - Tổng Quan Hệ Thống

**Tóm tắt**: Hệ thống ETL phiên bản v2 được thiết kế để chuẩn hóa luồng dữ liệu từ Production sang Datamart theo kiến trúc Modular, hướng đối tượng (OOP) và đảm bảo tính thống nhất giữa chế độ chạy Tự động (Auto) và Thủ công (Manual).

**Nguồn**: `raw/MASTER_KNOWLEDGE_BASE.md` (README.md section)

**Cập nhật lần cuối**: 2026-05-22

---

## 1. Mục Đích Hệ Thống

Hệ thống ETL v2 có 3 mục tiêu chính:
- **Chuẩn hóa luồng dữ liệu**: Từ Production (HIS) → Staging → Datamart
- **Kiến trúc Modular**: Tách riêng Dimension (full-load) và Fact (incremental)
- **An toàn dữ liệu**: Bảo vệ Production, cô lập cơ sở theo facility, chống lỗi dữ liệu lẫn

(nguồn: README.md)

## 2. Cấu Trúc Thư Mục Chính

```
ETL_Nano_Project_V2/
├── agents.md                    # Chiến lược điều phối
├── README.md                    # Hướng dẫn tổng quan
├── PROJECT_CHRONICLE.md         # Nhật ký thay đổi kỹ thuật
├── .env                         # Biến môi trường & kết nối DB
├── config/                      # Cấu hình hệ thống (tables.yaml)
├── docs/knowledge/              # Tầng tri thức GEM
│   ├── GEM_NAVIGATION.md
│   ├── GEM_GUIDE.md
│   ├── GEM_CODE_MAP.md
│   ├── GEM_CODE_SNIPPETS.md
│   ├── GEM_DATA_FLOW.md
│   ├── GEM_DB_SCHEMAS.md
│   ├── GEM_AUTO_PIPELINE.md
│   ├── GEM_DEPENDENCY_GRAPH.md
│   ├── GEM_ERROR_CONTEXT.md
│   ├── GEM_TECHNICAL_STANDARDS.md
│   └── GEM_SYNC_WORKFLOW.md
├── scripts/                     # Script vận hành
├── src/                         # Mã nguồn chính
│   ├── core/                    # Base classes, Logger, DB
│   ├── jobs/                    # Logic loader cụ thể
│   ├── ui/                      # Giao diện NiceGUI
│   └── db/templates/sql/        # SQL MERGE templates
├── tests/                       # Test scripts
└── .github/workflows/           # GitHub Actions
```

(nguồn: README.md)

## 3. Các Tầng Tri Thức (GEM Documents)

| Tên Tài Liệu | Mục Đích |
|---|---|
| **GEM_NAVIGATION.md** | Chỉ dẫn tìm kiếm tri thức |
| **GEM_GUIDE.md** | Thứ tự ưu tiên đọc tài liệu |
| **GEM_CODE_MAP.md** | Sơ đồ classes & functions |
| **GEM_CODE_SNIPPETS.md** | Thư viện mã mẫu chuẩn |
| **GEM_DATA_FLOW.md** | Luồng dữ liệu 3 chặng |
| **GEM_DB_SCHEMAS.md** | Cấu trúc bảng & schema |
| **GEM_AUTO_PIPELINE.md** | Engine chạy tự động |
| **GEM_DEPENDENCY_GRAPH.md** | Đồ thị phụ thuộc module |
| **GEM_ERROR_CONTEXT.md** | Mã lỗi & xử lý |
| **GEM_TECHNICAL_STANDARDS.md** | Tiêu chuẩn kỹ thuật |
| **GEM_SYNC_WORKFLOW.md** | Đồng bộ Diff giữa local & Drive |

(nguồn: README.md)

## 4. Vai Trò của Agent & Codex

Hệ thống hoạt động theo mô hình phối hợp:
- **Gem (Kiến trúc sư)**: Điều phối, ra lệnh dựa trên Tầng tri thức
- **Codex (Thợ code)**: Thực thi lập trình, sửa lỗi, cập nhật tài liệu

Quy trình bắt buộc:
1. Trước sửa code → đọc GEM_*.md liên quan
2. Thực hiện sửa đổi mã nguồn
3. Cập nhật Tầng tri thức (GEM_*.md)
4. Kết xuất báo cáo vào `REPORT_CHANGES.md`

(nguồn: agents.md)

## 5. Ngôn Ngữ & Quy Chuẩn

- **Ngôn ngữ**: Tiếng Việt rõ ràng, ngắn gọn
- **Hình thức**: Không sử dụng icon trong tài liệu .md
- **Kỹ thuật**: OOP, kế thừa từ Base Class tại `src/core/`

(nguồn: agents.md)

---

## Các Trang Liên Quan

- [[kien-truc-da-tang]] - Kiến trúc Multi-hop 3 chặng
- [[orchestrator-dong-bo]] - Điều phối tuần tự
- [[dimension-loader]] - Full-load Dimension
- [[fact-loader]] - Incremental Fact
- [[quan-ly-he-thong]] - Cấu hình & quản trị
- [[chinh-sach-bao-mat-db]] - Quy tắc bảo mật
- [[loi-va-khac-phuc]] - Lỗi & giải pháp
