# LLM Wiki

Một cơ sở tri thức cá nhân được bảo trì bởi Codex.
Dựa trên mô hình LLM Wiki của Andrej Karpathy.

## Mục đích

Wiki này là một cơ sở tri thức có cấu trúc, được liên kết với nhau.
Codex bảo trì wiki. Con người tuyển chọn nguồn dữ liệu, đặt câu hỏi và định hướng phân tích.

## Cấu trúc thư mục

```text
raw/          -- tài liệu nguồn (bất biến -- tuyệt đối không chỉnh sửa)
wiki/         -- các trang markdown do Claude bảo trì
wiki/index.md -- mục lục cho toàn bộ wiki
wiki/log.md   -- nhật ký chỉ ghi thêm (append-only) của tất cả các thao tác
```

## Quy trình Ingest (Nạp dữ liệu)

Khi người dùng thêm một nguồn mới vào `raw/` và yêu cầu bạn nạp (ingest) nó:

1. Đọc toàn bộ tài liệu nguồn
2. Thảo luận các điểm chính yếu với người dùng trước khi viết bất cứ điều gì
3. Tạo một trang tóm tắt trong `wiki/` được đặt tên theo nguồn
4. Tạo hoặc cập nhật các trang khái niệm cho từng ý tưởng hoặc thực thể chính
5. Thêm các liên kết wiki (`[[tên-trang]]`) để kết nối các trang liên quan
6. Cập nhật `wiki/index.md` với các trang mới và mô tả tóm tắt trên một dòng
7. Thêm một mục vào `wiki/log.md` với ngày tháng, tên nguồn và những gì đã thay đổi

Một nguồn duy nhất có thể tác động đến 10-15 trang wiki. Điều đó là bình thường.

## Định dạng trang

Mọi trang wiki cần tuân theo cấu trúc sau:

```markdown
# Tên Trang

**Tóm tắt**: Một đến hai câu mô tả trang này.

**Nguồn**: Danh sách các file nguồn gốc mà trang này trích xuất thông tin.

**Cập nhật lần cuối**: Ngày cập nhật gần nhất.

---

Nội dung chính nằm ở đây. Sử dụng các tiêu đề rõ ràng và các đoạn văn ngắn.

Liên kết đến các khái niệm liên quan bằng cách sử dụng [[liên-kết-wiki]] xuyên suốt văn bản.

## Các trang liên quan

- [[khái-niệm-liên-quan-1]]
- [[khái-niệm-liên-quan-2]]
```

## Quy tắc trích dẫn

- Mọi khẳng định thực tế đều phải tham chiếu đến file nguồn của nó
- Sử dụng định dạng `(nguồn: ten_file.pdf)` sau mỗi khẳng định
- Nếu hai nguồn mâu thuẫn nhau, hãy ghi chú rõ ràng sự mâu thuẫn đó
- Nếu một khẳng định không có nguồn, hãy đánh dấu là cần xác minh

## Trả lời câu hỏi

Khi người dùng đặt câu hỏi:

1. Đọc `wiki/index.md` trước để tìm các trang liên quan
2. Đọc các trang đó và tổng hợp thành một câu trả lời
3. Trích dẫn các trang wiki cụ thể trong câu trả lời của bạn
4. Nếu câu trả lời không có trong wiki, hãy nói rõ điều đó
5. Nếu câu trả lời có giá trị, hãy đề xuất lưu nó thành một trang wiki mới

Những câu trả lời tốt nên được lưu lại vào wiki để chúng tích lũy dần theo thời gian.

## Kiểm tra (Lint)

Khi người dùng yêu cầu bạn kiểm tra (lint) hoặc đánh giá (audit) wiki:

- Kiểm tra các mâu thuẫn giữa các trang
- Tìm các trang mồ côi (không có liên kết trỏ đến từ các trang khác)
- Xác định các khái niệm được nhắc đến trong các trang nhưng lại chưa có trang riêng
- Gắn cờ các khẳng định có thể đã lỗi thời dựa trên các nguồn mới hơn
- Kiểm tra xem tất cả các trang có tuân thủ định dạng trang ở trên hay không
- Báo cáo các phát hiện dưới dạng danh sách đánh số kèm theo các đề xuất sửa lỗi

## Quy tắc

- Tuyệt đối không chỉnh sửa bất cứ thứ gì trong thư mục `raw/`
- Luôn cập nhật `wiki/index.md` và `wiki/log.md` sau khi có thay đổi
- Giữ tên trang ở dạng chữ thường và dùng dấu gạch ngang (ví dụ: `machine-learning.md`)
- Viết bằng ngôn ngữ rõ ràng, đơn giản
- Khi không chắc chắn về cách phân loại một thứ gì đó, hãy hỏi người dùng