# Tổng quan hệ thống WebApp ISOFH - BV Mắt Quốc tế Việt Nga

**Tóm tắt**: Hệ thống Web/App cho phép người bệnh tra cứu hồ sơ sức khỏe, kết quả khám chữa bệnh và đặt lịch khám trực tuyến tại Bệnh viện Mắt Quốc tế Việt Nga, do ISOFH triển khai.

**Nguồn**: 01_bblv_viet_nga_2025_04_03.md, 06_bblv_isofh_bv_mat_qt_viet_nga_2025_11_03.md, 00_manifest.md

**Cập nhật lần cuối**: 13/06/2026

---

## Mục tiêu

Cung cấp nền tảng Web/App cho phép người bệnh:
- Tra cứu hồ sơ sức khỏe và kết quả khám chữa bệnh (nguồn: 01_bblv_viet_nga_2025_04_03.md)
- Đặt lịch khám trực tuyến theo bác sĩ hoặc dịch vụ (nguồn: 04_plhd_app_03_03.md)
- Quản lý hồ sơ người thân và đặt khám cho người thân (nguồn: 04_plhd_app_03_03.md)
- Nhận thông báo kết quả khám, tin tức từ bệnh viện (nguồn: 04_plhd_app_03_03.md)

## Các bên liên quan

| Bên | Vai trò |
|-----|---------|
| **ISOFH** | Đơn vị triển khai, phát triển hệ thống Web/App |
| **BV Mắt Quốc tế Việt Nga** | Chủ đầu tư, cung cấp dữ liệu HIS, vận hành |
| **Người bệnh** | Người dùng cuối: tra cứu HSSK, đặt lịch khám |
| **CNTT bệnh viện** | Quản trị hệ thống Web Vendor, phân quyền người dùng |

## Các biên bản làm việc quan trọng

1. **03/04/2025** - Biên bản làm việc lần đầu: trao đổi yêu cầu triển khai Web/App, kết nối HIS (nguồn: 01_bblv_viet_nga_2025_04_03.md)
2. **03/11/2025** - Biên bản làm việc cập nhật: các nội dung thống nhất về Web/App/HIS (nguồn: 06_bblv_isofh_bv_mat_qt_viet_nga_2025_11_03.md)

## Kiến trúc tổng thể (sơ lược)

Hệ thống gồm:
- **Public DMZ** với HAProxy/Load Balancer tiếp nhận kết nối từ Internet
- **API Gateway** định tuyến request đến các service
- **Các service**: Authorization, Appointment, Schedule, User, Medical, Notification
- **Cơ sở dữ liệu**: PostgreSQL, Redis, MongoDB
- **Kết nối tới HIS/EMR** qua REST API
- **Giám sát**: Prometheus + MS Teams

Xem chi tiết tại [[kien-truc-he-thong]].

## Các trang liên quan

- [[kien-truc-he-thong]]
- [[dat-lich-kham]]
- [[ho-so-suc-khoe]]
- [[dong-bo-du-lieu-his]]
- [[xac-thuc-bao-mat]]
- [[web-vendor]]