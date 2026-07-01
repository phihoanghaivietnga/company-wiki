# Tóm tắt hệ thống: Personal AI Knowledge Assistant (LLM-Wiki nâng cấp)

## Bối cảnh
Phi đang xây dựng hệ thống quản lý tri thức cá nhân dựa trên LLM-Wiki của Karpathy, vận hành qua Cline + Qwen3, dùng GSD Core (BMAD Method) để build. Đã đi qua các bước: thiết kế gốc → mở rộng đa project → nâng cấp thành trợ lý AI chủ động (11 Wild Ideas) → bổ sung 2 luồng xử lý file trao đổi đối tác/liên phòng ban. Toàn bộ đã được đóng gói thành PRD đưa vào GSD Plan, hiện đang ở giai đoạn build/đã build xong AGENTS_MASTER.md + OPERATIONS_GUIDE.md.

## Cấu trúc thư mục tổng thể

```
company-wiki/
├── AGENTS.md (master schema, chung toàn hệ thống)
├── OPERATIONS_GUIDE.md (hướng dẫn vận hành hàng ngày)
├── _global/                    ← tri thức dùng chung toàn công ty
│   ├── raw/do/  raw/done/
│   └── wiki/
├── project-{X}/ (×10 project)
│   ├── AGENTS.md (override riêng project)
│   ├── raw/do/  raw/done/      ← tài liệu nguồn tĩnh, ingest 1 lần
│   ├── partner-tracker/        ← Excel trao đổi đối tác, cập nhật liên tục
│   │   ├── snapshots/  tracker.md
│   ├── agreements/              ← tài liệu đã chốt, làm bằng chứng pháp lý
│   │   ├── active/  archived/  index.md
│   └── wiki/
│       ├── index.md  log.md  thong_tin_mau_thuan.md
└── _system/
    ├── daily_scan_report.md
    ├── conflict_audit_log.md
    ├── daily_reflection_log.md
    └── query_log.md
```

## 12 Luồng vận hành đã thiết kế

1. **Daily Scan** — quét raw/do/ tất cả project, báo cáo, chờ xác nhận mới ingest
2. **Ingest** — đọc nguồn → check conflict trước khi ghi → tạo/update wiki page → move raw/do → raw/done
3. **Ingest Cập nhật (A→B)** — khi nguồn mới làm lỗi thời nguồn cũ, đánh dấu `[LỖI THỜI]` trong wiki, không xóa
4. **Lint/Audit** — kiểm tra trang mồ côi, broken link, thiếu nguồn, conflict chưa xử lý
5. **Contextual Search** — tìm theo ngữ cảnh, không chỉ keyword
6. **Local Reasoning** — xâu chuỗi nhiều trang wiki để suy luận câu trả lời chưa có sẵn
7. **Strategic Advisor** — phân tích đa góc nhìn, đề xuất giải pháp có trade-off rõ ràng
8. **Daily Companion** — tóm tắt tình trạng hệ thống đầu ngày, gợi ý ưu tiên
9. **Knowledge Interviewer** — AI chủ động phỏng vấn cuối ngày, khai thác bài học lưu vào wiki
10. **Conflict Feedback Loop** — sau khi user chốt conflict, AI update wiki + check cascade conflict + bắt buộc ghi WHY
11. **Partner Tracker Sync** — Excel trao đổi đối tác (nhiều dòng, có cột trạng thái/ý kiến), mỗi dòng = 1 issue có ID ổn định, theo dõi open/resolved
12. **Agreement Archive** — file docx/pdf/xlsx đã chốt, lưu nguyên bản làm bằng chứng, AI chỉ tóm tắt "về cái gì" không diễn giải nội dung pháp lý, active/archived khi hết hiệu lực

## Nguyên tắc cốt lõi xuyên suốt

- `raw/`, `agreements/active+archived` là **bất biến** — AI không bao giờ sửa nội dung gốc
- **Conflict luôn phải flag lên người dùng**, AI không tự quyết — định dạng báo cáo chuẩn có 4 lựa chọn (a/b/c/d)
- Mọi quyết định giải quyết conflict **bắt buộc ghi lý do (WHY)**, không chỉ ghi kết quả
- Phân biệt rõ 3 loại dữ liệu: tài liệu tĩnh (raw/wiki) — dữ liệu sống nhiều dòng (partner-tracker) — bằng chứng pháp lý (agreements)
- Tiêu chí phân loại nhanh: `.docx`/`.pdf` luôn → agreements; `.xlsx` còn update tiếp → partner-tracker, đã đóng băng → agreements

## Trạng thái hiện tại / việc đang làm dở
PRD_Luong11_12_Bo_Sung.md đã được tạo và đưa vào Cline để cập nhật AGENTS_MASTER.md + OPERATIONS_GUIDE.md với Luồng 11, 12. Cần xác nhận với Phi: việc cập nhật 2 file này đã chạy xong qua Cline/GSD chưa, và nếu xong thì có cần review nội dung output hay tạo thêm gì không (ví dụ: script khởi tạo cấu trúc thư mục cho 10 project, hoặc populate AGENTS.md riêng cho từng project).
