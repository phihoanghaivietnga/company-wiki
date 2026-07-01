# Thông tin mâu thuẫn — project-llm-wiki-webapp

Ghi nhận các mâu thuẫn phát hiện trong quá trình ingest tài liệu vào wiki của project này.
Đây là bản sao cục bộ — bản chính (toàn hệ thống) được lưu trong `_system/conflict_audit_log.md`.

---

## Conflict đang xử lý

### Conflict #1 — CCCD bắt buộc trên WebApp vs HIS không bắt buộc CCCD (ISSUE-010)
- **Ngày phát hiện**: 2026-07-01 (sync Partner Tracker)
- **Nguồn**: `partner-tracker/snapshots/issue_webapp_20260701.xlsx` — dòng ISSUE-010
- **Mô tả**: 
  - **HIS**: hiện tại KHÔNG bắt buộc nhập CCCD khi người bệnh đến khám
  - **WebApp**: bắt buộc nhập CCCD khi đăng ký tài khoản
  - Hệ quả: thông tin NB trên HIS và WebApp không bao giờ khớp với những NB không để lại CCCD trên HIS
- **Trang wiki liên quan**: [[dong-bo-du-lieu-his]], [[xac-thuc-bao-mat]], [[ho-so-suc-khoe]]
- **Tracker issue**: [[../partner-tracker/tracker.md|ISSUE-010]]
- **Ý kiến của VN (PHẢN HỒI CỦA CNTT)**:
  1. WebApp căn cứ vào đâu để bắt buộc nhập CCCD?
  2. Những trường hợp khi đi khám không để lại số CCCD thì NB làm thế nào để đăng ký trên ứng dụng?
- **Trạng thái**: `pending` — đang trao đổi với ISOFH

---

## Conflict đã xử lý

<!-- AI di chuyển conflict đã resolve xuống đây -->

---

## Quy tắc

- Mọi conflict phát hiện trong project này đều được ghi vào đây VÀ `_system/conflict_audit_log.md`
- Khi resolve: cập nhật cả 2 nơi
- **Bắt buộc ghi WHY** (lý do chọn phương án giải quyết)