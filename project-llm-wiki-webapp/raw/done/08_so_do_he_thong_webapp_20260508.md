# Sơ đồ hệ thống WebApp - 2026/05/08

## Metadata

- Source file: `So Do He Thong WebApp_20260508.png`
- Loại tài liệu: Ảnh sơ đồ hệ thống
- Chủ đề: Kiến trúc tổng thể WebApp

## Vai trò của sơ đồ

Sơ đồ này là nguồn trực quan để xác định:

- Các actor/hệ thống bên ngoài.
- Các thành phần trong hệ thống WebApp.
- Các vùng mạng hoặc lớp triển khai.
- Các kết nối giữa Web/App, API Gateway, service, database và HIS/EMR.

## Liên hệ với tài liệu triển khai

Sơ đồ cần được đọc cùng với:

- `03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md`
- `02_de_xuat_mo_hinh_trien_khai_webapp.md`

## Các thành phần kỳ vọng theo tài liệu mô tả hệ thống

Do nội dung ảnh cần được đối chiếu trực tiếp khi ingest bằng công cụ nhìn ảnh, danh sách dưới đây chỉ là các thành phần kỳ vọng từ tài liệu kiến trúc liên quan:

- Người dùng Internet: bệnh nhân, admin, người dùng hệ thống.
- Public DMZ.
- HAProxy/Load Balancer.
- Firewall.
- API Gateway.
- Authorization Service.
- Appointment Service.
- Schedule Service.
- User Service.
- Medical Service.
- Notification Service.
- PostgreSQL.
- Redis.
- MongoDB.
- HIS/EMR REST API.
- Backup/HA/DR nếu sơ đồ có thể hiện.

## Ghi chú ingest

Không được dùng danh sách kỳ vọng này để thay thế việc đọc ảnh gốc. Khi sinh use case hoặc kiến trúc chính thức, phải nhìn lại ảnh `So Do He Thong WebApp_20260508.png` để xác nhận tên thành phần, hướng mũi tên và boundary.