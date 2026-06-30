# Query Log

Nhật ký câu hỏi & câu trả lời có giá trị — ghi lại những Q&A có thể trở thành tri thức wiki nhưng chưa được lưu chính thức.
**Mục đích**: Tránh mất tri thức từ hội thoại. AI đề xuất lưu sau mỗi Q&A có giá trị.

---

## Cấu trúc một mục query

```markdown
### Q#{số} — {tóm tắt câu hỏi}
- **Ngày**: YYYY-MM-DD
- **Người hỏi**: Người dùng
- **Câu hỏi gốc**: "{nội dung câu hỏi}"
- **Tóm tắt câu trả lời**: [tóm tắt 2-3 câu]
- **Nguồn tham khảo**: 
  - [[ten-project/ten-trang-1]] — [thông tin gì được dùng]
  - [[ten-project/ten-trang-2]] — [thông tin gì được dùng]
- **Đã lưu vào wiki?**: Chưa / Đã lưu → [[ten-trang-wiki-mới]]
- **Lý do nếu chưa lưu**: [ví dụ: cần xác minh thêm / người dùng từ chối / chưa đến lúc]
```

---

## Danh sách query

<!-- AI thêm query mới vào đầu danh sách -->

---

## Ví dụ mẫu

### Q#001 — Ví dụ: Chiến lược pricing cho sản phẩm mới
- **Ngày**: 2026-06-29
- **Người hỏi**: Người dùng
- **Câu hỏi gốc**: "Nên định giá sản phẩm X theo mô hình freemium hay subscription?"
- **Tóm tắt câu trả lời**: Dựa trên dữ liệu từ 3 project, mô hình subscription phù hợp hơn vì: (1) thị trường mục tiêu là doanh nghiệp vừa, (2) chi phí vận hành cao cần revenue ổn định, (3) đối thủ đang dùng freemium và gặp vấn đề về conversion.
- **Nguồn tham khảo**: 
  - [[project-sniper/market-research]] — Dữ liệu thị trường và đối thủ
  - [[project-sniper/cost-analysis]] — Phân tích chi phí vận hành
  - [[_global/pricing-strategy]] — Kinh nghiệm pricing từ các project trước
- **Đã lưu vào wiki?**: Chưa
- **Lý do nếu chưa lưu**: Người dùng muốn thảo luận thêm với team trước khi chốt

---

## Quy tắc quản lý log

1. **AI chủ động đề xuất** sau mỗi câu hỏi có giá trị: "Lưu câu trả lời này vào wiki không?"
2. **Nếu người dùng đồng ý**: AI tạo/cập nhật trang wiki tương ứng → cập nhật query log: "Đã lưu → [[ten-trang]]"
3. **Nếu người dùng từ chối**: Ghi lại lý do ngắn gọn để tham khảo sau.
4. **Review định kỳ**: Trong Daily Scan, AI nên nhắc người dùng về các query "Chưa lưu" quá 7 ngày.
5. **Archive**: Nếu file >10KB, tạo `query_log_YYYY-MM.md` và bắt đầu file mới.