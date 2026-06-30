# Các vấn đề tồn đọng và câu hỏi mở

**Tóm tắt**: Tổng hợp các vấn đề cần làm rõ, các câu hỏi mở từ quá trình phân tích tài liệu dự án WebApp ISOFH - BV Mắt Quốc tế Việt Nga.

**Nguồn**: 10_open_questions.md, 20260611_0845_CacVanDeTonDong.md, 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md

**Cập nhật lần cuối**: 13/06/2026

---

## 1. Về nghiệp vụ đặt khám

1. Slot khám được tính theo bác sĩ, dịch vụ, phòng, chuyên khoa hay tổ hợp các yếu tố nào? (nguồn: 10_open_questions.md)
2. Quy tắc "đã đủ bệnh nhân" lấy từ đâu: cấu hình slot, lịch HIS, hay Web Vendor?
3. Đồng bộ lịch khám sang HIS là đồng bộ một chiều hay hai chiều?
4. Nếu đồng bộ HIS thất bại thì lịch trên App/Web xử lý thế nào?
5. Có cho phép hủy/đổi lịch không?
6. Có yêu cầu thanh toán trước khi xác nhận lịch không?

## 2. Về hồ sơ sức khỏe điện tử

1. Người bệnh xác thực bằng gì để xem hồ sơ sức khỏe? (nguồn: 10_open_questions.md)
2. Có yêu cầu OTP mỗi lần xem kết quả không?
3. Người thân được xem hồ sơ sức khỏe theo cơ chế ủy quyền nào?
4. Dữ liệu nào lấy từ HIS, dữ liệu nào lưu tại hệ thống ISOFH?
5. PACS được xem qua link, iframe, viewer tích hợp hay tải ảnh?
6. Có giới hạn thời gian lưu hoặc thời gian truy cập kết quả không?

## 3. Về OTP qua Zalo SMS

1. OTP hết hạn sau 3 phút - có cần cấu hình thời gian này không? (nguồn: 10_open_questions.md)
2. Giới hạn số lần gửi OTP là bao nhiêu?
3. Có cơ chế chống spam/rate limit không?
4. Trạng thái SMS từ đối tác gồm những trạng thái nào?

## 4. Về thanh toán

1. Webapp tra cứu hiện tại không có chức năng thanh toán trực tuyến (nguồn: 10_open_questions.md)

## 5. Về bảo mật

1. **MD5** đang được ghi trong tài liệu là yêu cầu thật hay thông tin cũ? Cần thay bằng thuật toán băm an toàn hơn? (nguồn: 10_open_questions.md)
2. Có yêu cầu 2FA cho admin không?
3. WAF có triển khai chính thức không?
4. Log truy cập dữ liệu y tế lưu bao lâu?
5. Có phân quyền chi tiết theo RBAC cho Web Vendor không?

## 6. Về vận hành

1. RTO/RPO mục tiêu là bao nhiêu? (nguồn: 10_open_questions.md)
2. Backup giữ 15 ngày đã đủ theo yêu cầu bệnh viện chưa?
3. Có môi trường staging/UAT không?
4. Có quy trình test restore định kỳ và biên bản test restore không?
5. Ai chịu trách nhiệm giám sát Prometheus/MS Teams?

## 7. Vấn đề tồn đọng từ ứng dụng

1. **Đăng nhập nhiều lần thì bị khóa tài khoản**: Cần làm rõ đăng nhập sai bao nhiêu lần thì bị khóa? (nguồn: 20260611_0845_CacVanDeTonDong.md)
2. Cần yêu cầu có chức năng mở khóa tài khoản (hiện đã có trên Web Vendor, cần kiểm tra lại quy trình)

## 8. Về use case

1. Có cần tách use case cho App người bệnh và Web Vendor không? (nguồn: 10_open_questions.md)
2. HIS/EMR là actor ngoài hay hệ thống nằm ngoài boundary tích hợp?
3. Payment Gateway và SMS Provider có cần thể hiện trong sơ đồ use case tổng quan không?
4. Có cần dùng include/extend theo mẫu nội bộ không?

## Các trang liên quan

- [[dat-lich-kham]]
- [[ho-so-suc-khoe]]
- [[xac-thuc-bao-mat]]
- [[zalo-sms]]
- [[web-vendor]]
- [[dong-bo-du-lieu-his]]