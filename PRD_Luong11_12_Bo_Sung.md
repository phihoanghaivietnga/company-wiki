# PRD Bổ Sung: Luồng 11 (Partner Tracker) & Luồng 12 (Agreement Archive)

**Phiên bản**: 1.0
**Ngày**: 2026-06-30
**Áp dụng cho**: AGENTS_MASTER.md (bổ sung, không thay thế các luồng đã có)

---

## 1. Bối cảnh

Hệ thống hiện tại (raw/ → wiki/) xử lý tốt tài liệu nguồn tĩnh, nhưng chưa xử lý được 2 loại tri thức phát sinh trong thực tế làm việc với đối tác và liên phòng ban:

1. **Trao đổi đang diễn ra** qua Excel/Google Sheet nhiều dòng, có cột trạng thái, ý kiến hai bên — nội dung sống, cập nhật liên tục
2. **Tài liệu đã thống nhất** (.docx, .pdf, .xlsx loại báo cáo đã chốt) — cần lưu làm bằng chứng/đối chứng, không được diễn giải sai lệch

---

## 2. Tiêu chí phân loại file đầu vào (áp dụng đầu tiên, trước khi chọn luồng)

Khi người dùng đưa vào 1 file `.docx`, `.pdf`, hoặc `.xlsx`, AI phải hỏi/xác định theo tiêu chí sau trước khi xử lý:

| Câu hỏi quyết định | Nếu CÓ | Nếu KHÔNG |
|---|---|---|
| File có còn được gửi version mới, cập nhật tiếp không? | → **Luồng 11 (Partner Tracker)** | → xét tiếp |
| File là bảng nhiều dòng có cột trạng thái/ý kiến hai bên? | → **Luồng 11** | → **Luồng 12 (Agreement Archive)** |
| File là văn bản đã chốt, không đổi nữa (.docx, .pdf, hoặc .xlsx báo cáo 1 lần)? | → **Luồng 12** | — |

**Quy tắc nhanh**: `.docx` và `.pdf` luôn đi Luồng 12. `.xlsx` thì xét: còn update tiếp → Luồng 11; đã đóng băng → Luồng 12.

---

## 3. Luồng 11 — Partner Tracker Sync

### 3.1 Mục đích
AI nắm được toàn bộ thông tin đang trao đổi với đối tác (lỗi, phát sinh mới, vấn đề cần bàn) ở mức độ chi tiết từng dòng, không chỉ tóm tắt chung chung.

### 3.2 Cấu trúc thư mục

```text
project-{X}/
└── partner-tracker/
    ├── snapshots/
    │   ├── 2026-06-29_partner-feedback.xlsx
    │   └── 2026-07-05_partner-feedback.xlsx
    └── tracker.md
```

- `snapshots/`: lưu nguyên bản file mỗi lần đối tác/người dùng gửi, đặt tên `{YYYY-MM-DD}_{ten-file-goc}`, **bất biến, không sửa**
- `tracker.md`: AI duy trì, tổng hợp từng dòng thành tri thức có cấu trúc, có thể search/reasoning được

### 3.3 Quy trình sync khi có file .xlsx mới

**Bước 1**: Lưu file vào `partner-tracker/snapshots/` với tên có ngày tháng.

**Bước 2**: AI đọc file, xác định các cột: Ý kiến của user, Ý kiến đối tác, Trạng thái, Loại vấn đề, Chức năng liên quan.

**Bước 3**: Diff với `tracker.md` hiện tại:
- Dòng chưa có ID → đây là issue mới, gán ID mới (`ISSUE-{số tăng dần}`), **ID không bao giờ đổi qua các lần sync**
- Dòng đã có ID nhưng trạng thái/nội dung thay đổi → cập nhật, giữ nguyên ID
- Dòng có trạng thái "Đã xử lý"/"Đóng" → chuyển từ mục "Đang mở" sang "Đã đóng" trong `tracker.md`

**Bước 4**: Nếu phát hiện dòng dữ liệu mơ hồ (ví dụ cột "Ý kiến của tao" trống nhưng trạng thái là "Đang xử lý") → **chủ động hỏi người dùng** trước khi ghi, không tự suy diễn.

**Bước 5**: Nếu issue liên quan đến chức năng đã có trang wiki → thêm link `[[trang]]`. Nếu nội dung đối tác phản hồi mâu thuẫn với spec đã ghi trong wiki → kích hoạt Luồng Xử lý Conflict (đã có trong AGENTS_MASTER.md).

**Bước 6**: Ghi 1 dòng vào `wiki/log.md` như ingest bình thường.

### 3.4 Format `tracker.md`

```markdown
# Partner Tracker — {Tên đối tác / dự án}

**Cập nhật lần cuối**: 2026-07-05 (snapshot: 2026-07-05_partner-feedback.xlsx)

---

## Đang mở (open)

### [ISSUE-001] Lỗi: Nút thanh toán không hoạt động trên Safari
- **Loại**: Lỗi
- **Chức năng**: Paywall checkout
- **Ý kiến đối tác**: Button không trigger Stripe sheet trên Safari iOS
- **Ý kiến của tao**: Cần test lại trên Safari 17+, nghi do CSS sticky bottom
- **Trạng thái**: Đang xử lý
- **Lần đầu xuất hiện**: 2026-06-29
- **Lần cập nhật gần nhất**: 2026-07-05
- **Liên kết**: [[paywall-ux]]

---

## Đã đóng (resolved)

### [ISSUE-000] Phát sinh mới: Thêm Google Pay
- **Trạng thái cuối**: Đã triển khai (2026-07-03)
- **Tóm tắt**: [nội dung đã xử lý xong]
```

---

## 4. Luồng 12 — Agreement Archive

### 4.1 Mục đích
Lưu trữ tài liệu đã thống nhất giữa các phòng ban/đối tác làm bằng chứng đối chứng pháp lý sau này. AI **không được diễn giải lại nội dung pháp lý bằng văn phong riêng** — chỉ tóm tắt "về cái gì" và trỏ về bản gốc.

### 4.2 Cấu trúc thư mục

```text
project-{X}/
└── agreements/
    ├── active/       ← Còn hiệu lực
    │   ├── spec-v2-confirmed.docx
    │   └── budget-approved.xlsx
    ├── archived/     ← Hết hiệu lực, vẫn giữ để đối chứng
    │   └── spec-v1-confirmed.docx
    └── index.md
```

### 4.3 Quy trình

**Khi có file mới được thống nhất**:
1. Copy nguyên file (giữ nguyên định dạng gốc, không convert) vào `agreements/active/`
2. AI thêm 1 dòng vào bảng "Active" trong `index.md`: tên file, ngày, tóm tắt 1 câu (về cái gì, không diễn giải nội dung), link wiki liên quan nếu có
3. Nếu file này thay thế 1 file cũ → chuyển file cũ sang `archived/`, cập nhật `index.md`

**Khi 1 file hết hiệu lực** (có bản mới thay thế, hoặc quyết định khác):
1. Move file từ `active/` sang `archived/`
2. Chuyển dòng tương ứng trong `index.md` từ bảng Active xuống bảng Archived, ghi rõ lý do và file thay thế (nếu có)

### 4.4 Format `index.md`

```markdown
# Agreement Index — project-{X}

## Active

| File | Ngày | Nội dung | Liên quan |
|---|---|---|---|
| spec-v2-confirmed.docx | 2026-06-20 | Spec paywall v2 đã chốt Product-Eng | [[paywall-ux]] |
| budget-approved.xlsx | 2026-07-01 | Ngân sách Q3 đã duyệt | — |

## Archived

| File | Ngày | Lý do archive | Thay thế bởi |
|---|---|---|---|
| spec-v1-confirmed.docx | 2026-05-10 | Đã chốt phiên bản mới | spec-v2-confirmed.docx |
```

### 4.5 Quy tắc tuyệt đối

- AI không bao giờ viết lại/diễn giải nội dung điều khoản, cam kết, số liệu trong file — chỉ mô tả chủ đề
- Không convert định dạng file gốc
- Không xóa file khỏi `archived/` dù đã hết hiệu lực

---

## 5. Cập nhật cần thiết trong AGENTS_MASTER.md

- Thêm mục "Tiêu chí phân loại file đầu vào" (mục 2 ở trên) vào đầu phần Quy trình Ingest, áp dụng trước khi vào Luồng 2
- Thêm Luồng 11 — Partner Tracker Sync (nội dung mục 3)
- Thêm Luồng 12 — Agreement Archive (nội dung mục 4)
- Cập nhật sơ đồ cấu trúc thư mục project ở đầu file để bao gồm `partner-tracker/` và `agreements/`

## 6. Cập nhật cần thiết trong OPERATIONS_GUIDE.md

Bổ sung quy trình hàng ngày cho 2 luồng mới:
- Cách đưa file Excel trao đổi đối tác vào hệ thống (đặt ở đâu, gõ lệnh gì)
- Cách đưa file docx/pdf/xlsx đã thống nhất vào hệ thống (đặt ở đâu, gõ lệnh gì)
- Cách hỏi AI về tình trạng các issue đang mở với đối tác
- Cách tra cứu agreement khi cần đối chứng
