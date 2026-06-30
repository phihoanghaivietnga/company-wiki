# Xác thực và bảo mật

**Tóm tắt**: Cơ chế xác thực, phân quyền và các biện pháp bảo mật cho hệ thống WebApp ISOFH, bao gồm đăng nhập, OTP, WAF, DMZ, SSL/TLS và quản lý phiên.

**Nguồn**: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md, 05_web_tra_cuu_hssk_bv_mat_vn.md

**Cập nhật lần cuối**: 13/06/2026

---

## Cơ chế xác thực

| Mục | Chi tiết |
|-----|----------|
| Loại xác thực | Username/Password, OTP SMS, Email, 2FA (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md) |
| Hỗ trợ | Đăng nhập bằng password hoặc OTP |
| Mã hóa mật khẩu | MD5 (cần rà soát lại theo chuẩn bảo mật hiện hành) |
| Thời gian đổi mật khẩu | 6 tháng |
| OTP qua Zalo SMS | OTP gửi qua tin nhắn Zalo, hết hạn sau 3 phút (nguồn: 10_open_questions.md) |

### Luồng đăng ký/đăng nhập

1. **Đăng ký**: Nhập SĐT + mật khẩu → Nhập OTP → Cập nhật hồ sơ (nguồn: 11_hdsd_web_tra_cuu.md)
2. **Đăng nhập**: Nhập SĐT + mật khẩu → Xác thực
3. **OTP**: Được gửi qua Zalo SMS khi đăng ký tài khoản, quên mật khẩu, hoặc thao tác nhạy cảm

## Quản lý phiên

| Loại | Thời gian timeout |
|------|-------------------|
| Vendor | 8 giờ (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md) |
| Người dùng | 30 phút |

- Có ghi log active của user (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)

## API Security

- Sau khi đăng nhập, hệ thống gửi access token và refresh token (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- App/Web dùng token để gọi API lấy dữ liệu

## Phân quyền

- Quyền của user tùy thuộc CNTT tự phân quyền các tính năng trên hệ thống (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- Tài khoản bị khóa không được truy cập hệ thống (nguồn: 07_hdsd_web_vendor_cntt.md)

## Khóa/Mở khóa tài khoản

- **Khóa tài khoản**: Chọn icon bật/tắt với tài khoản đang hoạt động, trạng thái chuyển thành "Đã khóa" (nguồn: 07_hdsd_web_vendor_cntt.md)
- **Mở khóa tài khoản**: Chọn icon bật/tắt với tài khoản đã khóa, trạng thái chuyển thành "Đang hoạt động"
- **Vấn đề tồn đọng**: Cần làm rõ đăng nhập sai bao nhiêu lần thì bị khóa (nguồn: 20260611_0845_CacVanDeTonDong.md)

## Bảo mật hồ sơ sức khỏe

- Hồ sơ sức khỏe là dữ liệu nhạy cảm, cần xác thực chặt (nguồn: 05_web_tra_cuu_hssk_bv_mat_vn.md)
- Cần ghi log truy cập hồ sơ sức khỏe
- Cần xác định chính sách che/masking dữ liệu cá nhân nếu hiển thị công khai trên màn hình

## Các vấn đề cần làm rõ

- **MD5**: Đây là thuật toán băm mật khẩu yếu, cần rà soát lại theo chuẩn bảo mật hiện hành (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)
- **2FA cho admin**: Có yêu cầu không? (nguồn: 10_open_questions.md)
- **WAF**: Có triển khai chính thức không? (đã có cảnh báo về độ trễ)
- **Log truy cập dữ liệu y tế**: Lưu bao lâu?
- **Có phân quyền chi tiết theo RBAC cho Web Vendor không?**
- **OTP rate limit**: Giới hạn số lần gửi OTP? Có cơ chế chống spam?

## Các trang liên quan

- [[kien-truc-he-thong]]
- [[ho-so-suc-khoe]]
- [[web-vendor]]
- [[zalo-sms]]
- [[cac-van-de-ton-dong]]