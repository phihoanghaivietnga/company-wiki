# Yêu cầu cung cấp tài liệu mô hình hệ thống Mobile App & Web trả kết quả

## 6. Cơ chế bảo mật

### WAF

- Có/Không và vị trí triển khai cần được xác định.
- Khuyến nghị bệnh viện sử dụng Cloud WAF như Cloudflare, Viettel Cloudrity để chống DDoS và tấn công thuộc Top 10 OWASP.
- Cần cân nhắc vì WAF có thể ảnh hưởng trải nghiệm người dùng và gây độ trễ khi tra cứu kết quả khám hoặc đặt khám.

### DMZ

- HAProxy đặt ở DMZ nhận kết nối từ Internet.
- Request được chuyển tiếp vào hệ thống ứng dụng ở mạng internal.

### API Security

- Khi khách hàng đăng nhập, hệ thống gửi access token và refresh token.
- App/Web dùng token để gọi API lấy dữ liệu.

### SSL/TLS

- ISOFH cần một domain có SSL cho website trả kết quả khám.

### Cơ chế phân quyền

- Quyền của user tùy thuộc CNTT tự phân quyền các tính năng trên hệ thống.

## 7. Cơ chế xác thực và phân quyền

| Mục | Nội dung yêu cầu | ISOFH phản hồi |
|---|---|---|
| Loại xác thực | Username/Password, OTP SMS, Email, 2FA | Hỗ trợ đăng nhập bằng password hoặc OTP |
| Chuẩn mật khẩu | Độ dài, ký tự đặc biệt, thời hạn đổi | Mã hóa MD5; thời gian đổi 6 tháng |
| Timeout phiên | Thời gian tự động đăng xuất | Vendor: 8 giờ; User: 30 phút |
| Ghi log đăng nhập | Có/Không, thời gian lưu trữ | Có ghi log active của user |

## 8. Sao lưu và khôi phục dữ liệu

| Mục | Nội dung yêu cầu | ISOFH phản hồi |
|---|---|---|
| Chu kỳ backup | Hàng ngày/giờ/tuần | Hàng ngày |
| Loại backup | Full / Incremental / Differential | Full |
| Lưu trữ backup | Onsite / Offsite / Cloud | Mặc định onsite; khuyến nghị thêm offsite/cloud |
| Thời gian lưu trữ | Số ngày/tháng/năm | 15 ngày |
| Kiểm thử khôi phục | Tần suất và quy trình test restore | 2 lần/năm; restore database từ bản backup latest |

## 9. Khả năng mở rộng và dự phòng

- Scale-up: tăng tài nguyên máy chủ hiện tại như CPU, memory, disk.
- Scale-out: thêm máy chủ.
- HA/DR: ISOFH khuyến nghị dự phòng nóng, mỗi thành phần gồm 2 máy chủ ảo để đảm bảo RTO/RPO thấp nhất có thể.
- Failover:
  - Nếu lỗi 1 máy chủ HAProxy/App, máy chủ HAProxy/App còn lại nhận tải ngay.
  - Nếu lỗi database chính, promote database phụ và sửa cấu hình app kết nối tới database phụ.

## 10. Giám sát và cảnh báo

| Mục | Nội dung yêu cầu | ISOFH phản hồi |
|---|---|---|
| Hệ thống giám sát | Zabbix/Prometheus/Khác | Prometheus |
| Cảnh báo sự cố | Email/SMS/Telegram | MS Teams |
| Log hệ thống | Định dạng, thời gian lưu trữ | rsyslog, lưu trữ 6 tháng |

## Ghi chú rủi ro

- MD5 là thuật toán băm mật khẩu yếu, nếu đây là yêu cầu triển khai thật thì cần rà soát lại theo chuẩn bảo mật hiện hành.
- WAF được khuyến nghị nhưng có cảnh báo về độ trễ, cần quyết định triển khai chính thức.
- Cần làm rõ RTO/RPO mục tiêu, cơ chế replication database và quy trình failover chi tiết.