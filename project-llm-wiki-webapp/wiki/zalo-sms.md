# Gửi Zalo SMS

**Tóm tắt**: Cơ chế gửi tin nhắn Zalo SMS qua Zalo bệnh viện, sử dụng cho OTP xác thực và thông báo đặt khám thành công.

**Nguồn**: 04_plhd_app_03_03.md, 10_open_questions.md

**Cập nhật lần cuối**: 13/06/2026

---

## Các tình huống gửi Zalo SMS

Hệ thống gửi Zalo SMS trong các trường hợp sau (nguồn: 04_plhd_app_03_03.md):

1. **Gửi OTP khi người bệnh đăng ký tài khoản**
2. **Gửi OTP khi người bệnh quên mật khẩu**
3. **Gửi thông tin tài khoản người bệnh khám sức khỏe**
4. **Gửi tin nhắn đặt khám thành công**

## Thông số OTP

- OTP được gửi qua tin nhắn Zalo (nguồn: 10_open_questions.md)
- OTP hết hạn sau 3 phút
- Cần làm rõ giới hạn số lần gửi OTP và cơ chế chống spam/rate limit

## Quản lý tin nhắn trên Web Vendor

- Lưu trữ tất cả tin nhắn đã gửi qua hệ thống ISOFH (nguồn: 07_hdsd_web_vendor_cntt.md)
- Trạng thái tin nhắn là trạng thái do đối tác SMS trả về khi ISOFH gửi tin nhắn

## Các câu hỏi mở

- OTP hết hạn sau 3 phút - có cần cấu hình thời gian này không? (nguồn: 10_open_questions.md)
- Giới hạn số lần gửi OTP là bao nhiêu?
- Có cơ chế chống spam/rate limit không?
- Trạng thái SMS từ đối tác gồm những trạng thái nào?

## Các trang liên quan

- [[xac-thuc-bao-mat]]
- [[web-vendor]]
- [[huong-dan-su-dung-web-vendor]]
- [[thong-bao-tin-tuc]]