# Open Questions - Raw Sources WebApp ISOFH

## Về phạm vi tài liệu

1. Cần đọc lại toàn văn các file biên bản làm việc để xác định phạm vi cam kết chính thức:
   - `1_BBLV Việt Nga 03_04_2025.docx`
   - `6_BBLV ISOFH - BV Mắt QT Việt Nga 03112025.docx`

2. Cần đọc lại toàn văn tài liệu đề xuất triển khai:
   - `2_[ISOFH] _De xuat mo hinh trien khai - WEB-APP.docx`

3. Cần đọc lại toàn văn tài liệu web tra cứu HSSK:
   - `5_Web tra cứu HSSK BV Mắt VN.docx`

## Về nghiệp vụ đặt khám

1. Slot khám được tính theo bác sĩ, dịch vụ, phòng, chuyên khoa hay tổ hợp các yếu tố nào?
2. Quy tắc “đã đủ bệnh nhân” lấy từ đâu: cấu hình slot, lịch HIS, hay Web Vendor?
3. Đồng bộ lịch khám sang HIS là đồng bộ một chiều hay hai chiều?
4. Nếu đồng bộ HIS thất bại thì lịch trên App/Web xử lý thế nào?
5. Có cho phép hủy/đổi lịch không?
6. Có yêu cầu thanh toán trước khi xác nhận lịch không?

## Về hồ sơ sức khỏe điện tử

1. Người bệnh xác thực bằng gì để xem hồ sơ sức khỏe?
2. Có yêu cầu OTP mỗi lần xem kết quả không?
3. Người thân được xem hồ sơ sức khỏe theo cơ chế ủy quyền nào?
4. Dữ liệu nào lấy từ HIS, dữ liệu nào lưu tại hệ thống ISOFH?
5. PACS được xem qua link, iframe, viewer tích hợp hay tải ảnh?
6. Có giới hạn thời gian lưu hoặc thời gian truy cập kết quả không?

## Về OTP qua Zalo SMS

1. OTP được gửi qua tin nhắn Zalo
2. OTP hết hạn sau 3 phút
3. Giới hạn số lần gửi OTP là bao nhiêu?
4. Có cơ chế chống spam/rate limit không?
5. Trạng thái SMS từ đối tác gồm những trạng thái nào?

## Về thanh toán

1. Webapp tra cứu hiện tại không có chức năng thanh toán trực tuyến

## Về bảo mật

1. MD5 đang được ghi trong tài liệu là yêu cầu thật hay thông tin cũ?
2. Có bắt buộc thay MD5 bằng thuật toán băm mật khẩu an toàn hơn không?
3. Có yêu cầu 2FA cho admin không?
4. WAF có triển khai chính thức không?
5. Log truy cập dữ liệu y tế lưu bao lâu?
6. Có phân quyền chi tiết theo RBAC cho Web Vendor không?

## Về vận hành

1. RTO/RPO mục tiêu là bao nhiêu?
2. Backup giữ 15 ngày đã đủ theo yêu cầu bệnh viện chưa?
3. Có môi trường staging/UAT không?
4. Có quy trình test restore định kỳ và biên bản test restore không?
5. Ai chịu trách nhiệm giám sát Prometheus/MS Teams?

## Về use case

1. Có cần tách use case cho App người bệnh và Web Vendor không?
2. HIS/EMR là actor ngoài hay hệ thống nằm ngoài boundary tích hợp?
3. Payment Gateway và SMS Provider có cần thể hiện trong sơ đồ use case tổng quan không?
4. Có cần dùng include/extend theo mẫu nội bộ trong `usecase_mau.jpg` không?