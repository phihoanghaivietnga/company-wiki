# Kiến trúc hệ thống WebApp

**Tóm tắt**: Kiến trúc tổng thể hệ thống WebApp ISOFH bao gồm các vùng mạng, thành phần hệ thống, cơ chế chuyển tiếp request, kết nối HIS và các yêu cầu vận hành.

**Nguồn**: 02_de_xuat_mo_hinh_trien_khai_webapp.md, 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md, 08_so_do_he_thong_webapp_20260508.md

**Cập nhật lần cuối**: 13/06/2026

---

## Sơ đồ kiến trúc tổng thể

```
Internet (Người dùng: bệnh nhân, admin)
    │
    ▼
┌─────────────────────┐
│   Public DMZ        │
│   HAProxy/LB        │
└────────┬────────────┘
         │
    ┌────▼────┐
    │ Firewall │
    └────┬────┘
         │
┌────────▼────────────┐
│   API Gateway       │
└────────┬────────────┘
         │
         ├──────────────────────────────────┐
         │                                  │
┌────────▼────────┐            ┌────────────▼──────────┐
│ Authorization   │            │  Appointment Service  │
│ Service         │            │  Schedule Service     │
│ User Service    │            │  Medical Service      │
│ Notification    │            │  ...                  │
│ Service         │            │                       │
└────────┬────────┘            └────────────┬──────────┘
         │                                  │
         ▼                                  ▼
┌─────────────────┐            ┌──────────────────────┐
│   PostgreSQL    │            │   MongoDB / Redis    │
│   (Dữ liệu      │            │   (Cache, dữ liệu    │
│    chính)       │            │    phi cấu trúc)     │
└─────────────────┘            └──────────────────────┘
         │
         ▼
┌─────────────────┐
│  HIS/EMR        │
│  REST API       │
└─────────────────┘
```

## Các thành phần hệ thống

### Vùng Public/DMZ
- **HAProxy/Load Balancer**: Đặt ở DMZ, nhận kết nối từ Internet, chuyển tiếp request vào hệ thống ứng dụng ở mạng internal (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- **Firewall**: Phân cách giữa DMZ và mạng internal

### API Gateway
- Định tuyến request từ client đến các service tương ứng
- Xác thực token, kiểm tra quyền truy cập

### Application Services
| Service | Chức năng |
|---------|-----------|
| **Authorization Service** | Xác thực, phân quyền, quản lý token |
| **User Service** | Quản lý thông tin người dùng, hồ sơ bệnh nhân |
| **Appointment Service** | Quản lý lịch hẹn, đặt khám |
| **Schedule Service** | Quản lý lịch làm việc bác sĩ, slot khám |
| **Medical Service** | Quản lý hồ sơ sức khỏe, kết quả khám |
| **Notification Service** | Gửi thông báo, SMS, Zalo |

### Cơ sở dữ liệu
- **PostgreSQL**: Dữ liệu chính của hệ thống (nguồn: 08_so_do_he_thong_webapp_20260508.md)
- **Redis**: Cache, session management
- **MongoDB**: Dữ liệu phi cấu trúc (nếu có)

### Kết nối HIS/EMR
- Hệ thống kết nối tới HIS/EMR qua REST API (nguồn: 08_so_do_he_thong_webapp_20260508.md)
- Chi tiết đồng bộ xem tại [[dong-bo-du-lieu-his]]

## Bảo mật

### WAF (Web Application Firewall)
- Khuyến nghị sử dụng Cloud WAF (Cloudflare, Viettel Cloudrity) để chống DDoS và tấn công Top 10 OWASP (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- Cần cân nhắc vì WAF có thể ảnh hưởng trải nghiệm người dùng và gây độ trễ

### SSL/TLS
- ISOFH cần một domain có SSL cho website trả kết quả khám (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)

### API Security
- Sử dụng access token và refresh token sau khi đăng nhập (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- App/Web dùng token để gọi API lấy dữ liệu

## Sao lưu và khôi phục

| Mục | Chi tiết |
|-----|----------|
| Chu kỳ backup | Hàng ngày (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md) |
| Loại backup | Full |
| Lưu trữ | Mặc định onsite; khuyến nghị thêm offsite/cloud |
| Thời gian lưu | 15 ngày |
| Kiểm thử khôi phục | 2 lần/năm; restore database từ bản backup latest |

## Khả năng mở rộng và dự phòng

- **Scale-up**: Tăng tài nguyên máy chủ hiện tại (CPU, memory, disk) (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- **Scale-out**: Thêm máy chủ
- **HA/DR**: Khuyến nghị dự phòng nóng, mỗi thành phần gồm 2 máy chủ ảo
- **Failover**:
  - Nếu lỗi 1 máy chủ HAProxy/App, máy chủ còn lại nhận tải ngay
  - Nếu lỗi database chính, promote database phụ và sửa cấu hình app kết nối tới database phụ

## Giám sát và cảnh báo

| Mục | Chi tiết |
|-----|----------|
| Hệ thống giám sát | Prometheus (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md) |
| Cảnh báo sự cố | MS Teams |
| Log hệ thống | rsyslog, lưu trữ 6 tháng |

## Các trang liên quan

- [[tong-quan-he-thong]]
- [[dong-bo-du-lieu-his]]
- [[xac-thuc-bao-mat]]
- [[web-vendor]]