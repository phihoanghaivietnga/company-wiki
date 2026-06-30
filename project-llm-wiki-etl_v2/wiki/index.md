# Chỉ Mục Chính (Index)

**Wiki ETL V2** - Cơ sở tri thức tập trung cho hệ thống ETL Nano Project v2

---

## 🎯 Bắt Đầu Nhanh

1. **Muốn hiểu tổng quát hệ thống?** → [[etl-v2-tong-quan]]
2. **Muốn biết kiến trúc 3 chặng?** → [[kien-truc-da-tang]]
3. **Muốn chạy thủ công?** → [[huong-dan-manual-runner]]
4. **Muốn hiểu vận hành tự động?** → [[huong-dan-auto-runner]]
5. **Gặp lỗi?** → [[loi-va-khac-phuc]]

---

## 📚 Tất Cả Trang Wiki

### 1. Tổng Quan & Kiến Trúc
- [[etl-v2-tong-quan]] - Giới thiệu hệ thống, mục đích, cấu trúc thư mục
- [[kien-truc-da-tang]] - Kiến trúc 3-hop (Production → Staging → ODS → Datamart)

### 2. Điều Phối & Vận Hành
- [[orchestrator-dong-bo]] - SyncOrchestrator: điều phối tuần tự, selective sync
- [[huong-dan-auto-runner]] - Hướng dẫn chạy tự động (cron/GitHub Actions)
- [[huong-dan-manual-runner]] - Hướng dẫn chạy thủ công (UI browser)

### 3. Loader Chi Tiết
- [[dimension-loader]] - Nạp Dimension full-load 2-hop
- [[fact-loader]] - Nạp Fact incremental 3-hop với D-3 safeguard

### 4. Cấu Hình & Quản Trị
- [[quan-ly-he-thong]] - YAML config, facility management, mở rộng hệ thống
- [[chinh-sach-bao-mat-db]] - Quy tắc bảo mật DB, guard rails, log sanitization

### 5. Lịch Sử & Troubleshoot
- [[quyet-dinh-kien-truc-40adr]] - 40 quyết định kiến trúc (ADRs) từ 2026-05-09 đến 2026-05-21
- [[loi-va-khac-phuc]] - Lỗi thường gặp, nguyên nhân, cách khắc phục

---

## 🔗 Bản Đồ Liên Kết (Link Graph)

```
etl-v2-tong-quan
├─→ kien-truc-da-tang
│   ├─→ dimension-loader
│   ├─→ fact-loader
│   └─→ orchestrator-dong-bo
│       ├─→ huong-dan-auto-runner
│       ├─→ quan-ly-he-thong
│       └─→ chinh-sach-bao-mat-db
│
└─→ huong-dan-manual-runner
    ├─→ dimension-loader
    ├─→ fact-loader
    └─→ loi-va-khac-phuc
        └─→ quyet-dinh-kien-truc-40adr
```

---

## 📖 Quy Trình Đọc Tài Liệu

### Dành cho Người Bắt Đầu
1. [[etl-v2-tong-quan]] - Hiểu mục đích & cấu trúc
2. [[kien-truc-da-tang]] - Nắm kiến trúc 3 chặng
3. [[huong-dan-manual-runner]] - Thử chạy thủ công
4. [[loi-va-khac-phuc]] - Biết cách xử lý khi lỗi

### Dành cho Developer
1. [[etl-v2-tong-quan]] - Tổng quan
2. [[kien-truc-da-tang]] - Kiến trúc chi tiết
3. [[dimension-loader]] + [[fact-loader]] - Logic loader
4. [[quan-ly-he-thong]] - Cấu hình YAML
5. [[quyet-dinh-kien-truc-40adr]] - Hiểu lý do thiết kế
6. [[loi-va-khac-phuc]] - Troubleshoot

### Dành cho Ops/DevOps
1. [[huong-dan-auto-runner]] - Cấu hình scheduler
2. [[quan-ly-he-thong]] - Quản lý facility & config
3. [[chinh-sach-bao-mat-db]] - Bảo mật DB
4. [[loi-va-khac-phuc]] - Debug khi lỗi

---

## 🔍 Tra Cứu Theo Chủ Đề

| Chủ Đề | Trang Liên Quan |
|---|---|
| **Kiến Trúc** | [[kien-truc-da-tang]], [[quyet-dinh-kien-truc-40adr]] |
| **Nạp Dữ Liệu** | [[dimension-loader]], [[fact-loader]] |
| **Vận Hành** | [[orchestrator-dong-bo]], [[huong-dan-auto-runner]], [[huong-dan-manual-runner]] |
| **Cấu Hình** | [[quan-ly-he-thong]] |
| **Bảo Mật** | [[chinh-sach-bao-mat-db]] |
| **Debug** | [[loi-va-khac-phuc]] |
| **Lịch Sử** | [[quyet-dinh-kien-truc-40adr]] |

---

## ✅ Checklist Sử Dụng

### Lần Đầu Setup Hệ Thống
- [ ] Đọc [[etl-v2-tong-quan]]
- [ ] Đọc [[kien-truc-da-tang]]
- [ ] Cấu hình [[quan-ly-he-thong]]
- [ ] Setup [[huong-dan-auto-runner]]
- [ ] Test [[huong-dan-manual-runner]]

### Trước Khi Deploy Production
- [ ] Review [[chinh-sach-bao-mat-db]]
- [ ] Xem [[quyet-dinh-kien-truc-40adr]] (hiểu thiết kế)
- [ ] Bookmark [[loi-va-khac-phuc]]

### Khi Gặp Lỗi
- [ ] Tra cứu [[loi-va-khac-phuc]]
- [ ] Nếu lỗi liên quan loader → [[dimension-loader]] hoặc [[fact-loader]]
- [ ] Nếu lỗi vận hành → [[orchestrator-dong-bo]]
- [ ] Nếu lỗi DB → [[chinh-sach-bao-mat-db]]

---

## 📝 Thông Tin Meta

- **Ngôn Ngữ**: Tiếng Việt
- **Phiên Bản Wiki**: 1.0 (2026-05-22)
- **Cập Nhật Lần Cuối**: 2026-05-22
- **Số Trang**: 11
- **Số ADR**: 40 (lịch sử thiết kế)

---

## 🤝 Đóng Góp

Để cập nhật wiki:
1. Sửa markdown file trong `wiki/`
2. Cập nhật `wiki/index.md` nếu thêm/xóa trang
3. Ghi dòng mới vào `wiki/log.md` (append-only)

---

## 📞 Liên Hệ

- **Admin Wiki**: Codex
- **Kiến Trúc sư**: Gem
- **Team**: Codex + Gem
