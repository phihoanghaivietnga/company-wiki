# Conflict Audit Log

Nhật ký kiểm toán mâu thuẫn tri thức — ghi nhận mọi conflict được phát hiện trong toàn hệ thống wiki.
**Nguyên tắc bắt buộc**: Mọi quyết định giải quyết conflict phải ghi rõ **WHY** (lý do). Trường `lý_do` không được để trống.

---

## 2026-07-01 11:24 — Luồng 4 — Lint project-test-kim

### Tổng quan
- **Số file đã quét:** 9 (5 wiki + 2 raw/done + 1 tracker + 1 agreements)
- **Số issue phát hiện:** 8
  - Conflict: 1
  - Duplicate: 0
  - Stale: 1
  - Gap: 5
  - Broken link: 1
  - Missing link: 0
- **Thời gian thực hiện:** ~10 phút

### Chi tiết từng issue

#### Conflict #1: note_rac.md chỉ có 1 dòng "Nội dung test" — thiếu nội dung thật
- **Loại:** Mâu thuẫn trạng thái / Thiếu hụt nội dung
- **Mô tả:** File `note_rac.md` chỉ chứa đúng 1 dòng "Nội dung test", không có nội dung thực tế. Trong khi đó `wiki/log.md` ghi nhận file này được tạo từ ingest `B-lich-release-update.md` và `index.md` liệt kê nó là một trang wiki chính thức.
- **File liên quan:**
  - `project-test-kim/wiki/note_rac.md` (toàn bộ): "Nội dung test"
  - `project-test-kim/wiki/index.md` (dòng 9-11): liệt kê [[note_rac.md|Note từ RAC]] như một trang có nội dung
  - `project-test-kim/wiki/log.md` (dòng 5-6): ghi nhận ingest B tạo note_rac
- **Đề xuất:** Khôi phục nội dung `note_rac.md` từ `raw/done/B-lich-release-update.md` (phản hồi RAC về delay đối tác Shopee/Lazada/Tiki, khuyến nghị dời release).
- **Mức độ:** High

#### Stale #1: lich-release-sniper.md có nội dung được đánh dấu [LỖI THỜI]
- **Loại:** Nội dung đánh dấu lỗi thời đã xử lý đúng
- **Mô tả:** `lich-release-sniper.md` dòng 11-12 đã đánh dấu `[LỖI THỜI]` cho nội dung cũ từ A (Q3/2026, push notification trước). Nội dung mới từ B (Q4/2026, paywall trước) đã thay thế. Đã xử lý đúng quy trình.
- **File liên quan:**
  - `project-test-kim/wiki/lich-release-sniper.md` (dòng 11-12)
- **Đề xuất:** Không cần xử lý — đã đánh dấu đúng chuẩn. Có thể cân nhắc xóa hẳn nội dung [LỖI THỜI] sau 30 ngày.
- **Mức độ:** Low (informational)

#### Gap #1: Broken link trong thong_tin_mau_thuan.md
- **Loại:** Broken link
- **Mô tả:** `thong_tin_mau_thuan.md` dòng 19 chứa link `[[trang-khong-ton-tai]]` — đây là broken link vì không có file wiki nào tên này.
- **File liên quan:**
  - `project-test-kim/wiki/thong_tin_mau_thuan.md` (dòng 19)
- **Đề xuất:** Xóa dòng 19 hoặc thay bằng link hợp lệ.
- **Mức độ:** Medium

#### Gap #2: log.md không có trong index.md
- **Loại:** Thiếu liên kết (missing from index)
- **Mô tả:** `wiki/log.md` tồn tại thực tế nhưng không được liệt kê trong `wiki/index.md`. File này là nhật ký ingest, cần được index hoặc ít nhất ghi chú rõ đây là file hệ thống.
- **File liên quan:**
  - `project-test-kim/wiki/index.md`
  - `project-test-kim/wiki/log.md`
- **Đề xuất:** Thêm dòng `[[log]]` vào index.md hoặc ghi chú "log.md là nhật ký hệ thống".
- **Mức độ:** Low

#### Gap #3: index.md mô tả note_rac không khớp thực tế
- **Loại:** Thiếu chính xác
- **Mô tả:** `index.md` liệt kê `[[lich-release-sniper]]` với mô tả "Lịch release sản phẩm Sniper — Q4/2026 (dời do delay đối tác), ưu tiên paywall trước push notification". Nhưng không có dòng nào cho `note_rac.md` và `thong_tin_mau_thuan.md` — 2 trang có thật trong wiki nhưng bị thiếu trong bảng index.
- **File liên quan:**
  - `project-test-kim/wiki/index.md` (dòng 9-11): chỉ có 1 dòng duy nhất cho lich-release-sniper
- **Đề xuất:** Thêm dòng cho `note_rac.md` và `thong_tin_mau_thuan.md` vào bảng index, HOẶC cập nhật lại index cho đồng bộ với các file wiki thực tế.
- **Mức độ:** Medium

#### Gap #4: lich-release-sniper.md thiếu mục "Các trang liên quan" đầy đủ
- **Loại:** Thiếu liên kết chéo
- **Mô tả:** `lich-release-sniper.md` dòng 31-33 chỉ có "(chưa có trang liên quan nào)", nhưng thực tế có `thong_tin_mau_thuan.md` và `note_rac.md` đều liên quan đến conflict release Sniper. Nên link đến các trang này.
- **File liên quan:**
  - `project-test-kim/wiki/lich-release-sniper.md` (dòng 31-33)
  - `project-test-kim/wiki/thong_tin_mau_thuan.md` (có link đến lich-release-sniper)
  - `project-test-kim/wiki/note_rac.md` (từng có link đến lich-release-sniper)
- **Đề xuất:** Thêm `[[thong_tin_mau_thuan]]` và `[[note_rac]]` vào mục "Các trang liên quan".
- **Mức độ:** Low

#### Gap #5: note_rac.md không có nội dung thực — không thể đánh giá format
- **Loại:** Thiếu định dạng chuẩn (không thể đánh giá)
- **Mô tả:** `note_rac.md` chỉ có 1 dòng "Nội dung test", không thể kiểm tra format chuẩn (Tóm tắt, Nguồn, Cập nhật lần cuối, Các trang liên quan).
- **File liên quan:**
  - `project-test-kim/wiki/note_rac.md`
- **Đề xuất:** Khôi phục nội dung đầy đủ — xem Conflict #1 ở trên.
- **Mức độ:** High

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

### Conflict #002 — CCCD bắt buộc trên WebApp vs HIS không bắt buộc CCCD (ISSUE-010)
- **Ngày phát hiện**: 2026-07-01
- **Project**: project-llm-wiki-webapp
- **Nguồn gốc**: `partner-tracker/snapshots/issue_webapp_20260701.xlsx` — dòng ISSUE-010 (sync Partner Tracker)
- **Mô tả**:
  - **HIS**: hiện tại KHÔNG bắt buộc nhập CCCD khi người bệnh đến khám
  - **WebApp**: bắt buộc nhập CCCD khi đăng ký tài khoản
  - Hệ quả: thông tin NB trên HIS và WebApp không bao giờ khớp với những NB không để lại CCCD trên HIS
- **Trang wiki liên quan**: [[project-llm-wiki-webapp/dong-bo-du-lieu-his]], [[project-llm-wiki-webapp/xac-thuc-bao-mat]], [[project-llm-wiki-webapp/ho-so-suc-khoe]]
- **Ý kiến VN (PHẢN HỒI CỦA CNTT)**:
  1. WebApp căn cứ vào đâu để bắt buộc nhập CCCD?
  2. Những trường hợp khi đi khám không để lại số CCCD thì NB làm thế nào để đăng ký trên ứng dụng?
- **Mức độ**: Major
- **Đề xuất xử lý**: Cần thống nhất với ISOFH về phương án xử lý cho NB không có CCCD trên HIS. Các phương án khả dĩ: (a) HIS cập nhật bắt buộc CCCD, (b) WebApp cho phép đăng ký không CCCD và dùng mã NB thay thế, (c) cung cấp cơ chế xác thực thay thế.
- **Trạng thái**: `pending` — đang trao đổi với ISOFH

---

### Đã xử lý (Resolved)

### Conflict #001 — Mâu thuẫn lịch release Sniper (thời điểm + thứ tự ưu tiên)
- **Ngày phát hiện**: 2026-07-01
- **Ngày giải quyết**: 2026-07-01
- **Trang wiki**: [[project-test-kim/lich-release-sniper]]
- **Nguồn cũ**: `A-lich-release.md` (ingest: 2026-07-01) — Release Q3/2026, ưu tiên push notification trước paywall
- **Nguồn mới**: `B-lich-release-update.md` — Release Q4/2026 (delay từ đối tác thanh toán), ưu tiên paywall trước push notification
- **Mức độ**: Major
- **Quyết định**: (a) B thay thế hoàn toàn A — cập nhật toàn bộ thời điểm release và thứ tự ưu tiên theo B
- **Lý do (WHY)**: Vì B là thông tin mới nhất, đã xác nhận với đối tác thanh toán
- **Cập nhật wiki**: `lich-release-sniper.md` — đánh dấu nội dung A là LỖI THỜI, thay bằng nội dung B. Cập nhật nguồn thành B-lich-release-update.md. Metadata file A đánh dấu fully_superseded.
- **Cascade check**: Không phát sinh conflict mới.
- **Trạng thái**: `resolved`

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