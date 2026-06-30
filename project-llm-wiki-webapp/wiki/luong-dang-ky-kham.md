# Luồng đăng ký khám - Duyệt - Thông báo kết quả

**Tóm tắt**: Luồng xử lý từ khi người bệnh đặt lịch khám đến khi nhân viên y tế duyệt và đồng bộ sang HIS.

**Nguồn**: 20260612_0800_LuongDangKyKham.md

**Cập nhật lần cuối**: 13/06/2026

---

## Luồng tổng thể

```
Người bệnh đặt lịch khám
    │
    ▼
Nhân viên y tế (Web Vendor) duyệt / từ chối lịch
    │
    ▼
Vào 00h hàng ngày → đồng bộ lịch hẹn đã duyệt sang HIS
```

(nguồn: 20260612_0800_LuongDangKyKham.md)

## Chi tiết các bước

### Bước 1: Người bệnh đặt lịch
- Người bệnh thực hiện đặt lịch trên Web/App (theo bác sĩ hoặc theo dịch vụ)
- Xem chi tiết tại [[dat-lich-kham]]

### Bước 2: Nhân viên y tế duyệt/từ chối
- Nhân viên y tế vào Web Vendor kiểm tra lịch đăng ký
- Duyệt hoặc từ chối lịch hẹn
- Nếu từ chối, cần có lý do (cần xác nhận)

### Bước 3: Đồng bộ sang HIS
- Vào 00h hàng ngày, hệ thống tự động đồng bộ các lịch hẹn đã duyệt sang HIS
- Dữ liệu đồng bộ bao gồm thông tin bệnh nhân, người giám hộ, lịch khám, dịch vụ khám
- Xem chi tiết tại [[dong-bo-du-lieu-his]]

## Các trang liên quan

- [[dat-lich-kham]]
- [[dong-bo-du-lieu-his]]
- [[web-vendor]]