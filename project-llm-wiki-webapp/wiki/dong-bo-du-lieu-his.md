# Đồng bộ dữ liệu giữa WebApp và HIS

**Tóm tắt**: Cơ chế đồng bộ dữ liệu giữa hệ thống WebApp ISOFH và hệ thống HIS của bệnh viện, bao gồm dữ liệu đặt khám đẩy sang HIS, danh mục đồng bộ từ HIS, và kết quả khám từ HIS gửi về WebApp.

**Nguồn**: 20260514_0830_DuLieuDongBo_WebApp_His.md, 04_plhd_app_03_03.md, 20260619_luong_dong_bo_du_lieu_webapp_his.md

**Cập nhật lần cuối**: 23/06/2026

---

## Luồng đồng bộ tổng thể

```
WebApp                              HIS
  │                                   │
  ├── Đẩy lịch đặt khám ─────────────►│  (00h hàng ngày)
  │                                   │
  │◄── Gửi kết quả khám ──────────────┤  (khi có KQ mới)
  │                                   │
  │◄── Danh mục (khoa, BS, dịch vụ) ──┤  (đồng bộ định kỳ)
```

## 1. Dữ liệu đặt khám đẩy sang HIS

Khi lịch hẹn được duyệt, hệ thống đẩy các thông tin sau sang HIS (nguồn: 20260514_0830_DuLieuDongBo_WebApp_His.md):

**Thông tin bệnh nhân:**
- Họ tên, giới tính, ngày sinh
- Số điện thoại
- CCCD/Giấy tờ, loại giấy tờ
- Địa chỉ chi tiết

**Thông tin người giám hộ** (bắt buộc với NB dưới 14 tuổi)
- Họ tên, SĐT, CCCD/Giấy tờ

**Thông tin lịch khám:**
- Mã tham chiếu, lý do khám, ngày đăng ký

**Thông tin dịch vụ khám:**
- Service ID, mã dịch vụ, phòng khám, khoa
- Giờ khám, số thứ tự

## 2. Danh mục đồng bộ từ HIS

Cần đồng bộ danh mục từ HIS (nguồn: 20260514_0830_DuLieuDongBo_WebApp_His.md):
- Khoa, chuyên khoa
- Phòng
- Dịch vụ
- Bác sĩ
- Loại đối tượng
- Loại hình thanh toán

## 3. Điều kiện định danh bệnh nhân

Để tra cứu Hồ sơ khám từ HIS, hệ thống sử dụng các trường thông tin sau (nguồn: 20260619_luong_dong_bo_du_lieu_webapp_his.md):

| Trường | Ghi chú |
|--------|---------|
| Họ tên | Hệ thống tự động chuẩn hóa về chữ in HOA trước khi đối chiếu |
| Ngày tháng năm sinh | |
| Số điện thoại | |
| Giới tính | |
| CCCD | Không bắt buộc - người bệnh chưa có CCCD vẫn đồng bộ được dữ liệu |
| Mã bệnh nhân | |

**Lưu ý**: Hệ thống KHÔNG phân biệt chữ hoa/thường khi đối chiếu, do tên được chuẩn hóa tự động (ví dụ: "Thủy" và "Thủy" đều được xem là cùng một tên).

## 4. Cơ chế tra cứu và đồng bộ tự động

Để nhận được hồ sơ và kết quả khám từ HIS, người bệnh cần thực hiện tính năng **Tra cứu kết quả khám** trên App/Web:

1. **Lần đầu tiên**: Người bệnh tự thực hiện tra cứu để hệ thống liên kết và đồng bộ hồ sơ từ HIS
2. **Các lần sau**: Khi có kết quả khám mới trên HIS, hệ thống tự động trả kết quả về App/Web
3. **Tần suất đồng bộ**: Định kỳ 30 phút (hoặc giá trị cấu hình khác), hệ thống lấy toàn bộ thông tin hồ sơ khám từ database của các cơ sở vào cache của webapp

**Lưu ý quan trọng**: 
- Người bệnh tự thực hiện tra cứu, KHÔNG có chức năng đồng bộ từ phía quản trị/vendor
- Không có cơ chế đồng bộ tự động mà không có hành động tra cứu đầu tiên từ người bệnh

## 5. Kết quả khám từ HIS sang WebApp

- Khi có kết quả khám mới, HIS gửi cho app kết quả đó (nguồn: 20260514_0830_DuLieuDongBo_WebApp_His.md)
- App chỉ hiển thị, không lưu trữ dữ liệu gốc từ HIS
- Trên màn hình HIS, nhân viên y tế xem ở màn hình "Danh sách NB đã tiếp đón", cột Người chỉ định (ví dụ: App VRR)

## 6. Luồng đăng ký khám

```
Người bệnh đặt lịch → NVYT duyệt/từ chối (Web Vendor) → 00h đồng bộ sang HIS
```

(nguồn: 20260612_0800_LuongDangKyKham.md)

## 7. Hướng dẫn tạo dữ liệu test từ HIS về WebApp

Để kiểm tra thông tin người bệnh từ HIS trên WebApp (nguồn: 20260611_0900_CachTaoDuLieuTestTuProVeWeb.md):
1. Thêm người bệnh vào danh sách thành viên của tài khoản
2. Vào HIS tạm thời chỉnh SĐT của người bệnh thành SĐT của bạn
3. Thực hiện tra cứu kết quả khám trên WebApp
4. Sau khi tra cứu được kết quả, vào HIS chỉnh lại SĐT của người bệnh về đúng thông tin ban đầu

## Các vấn đề cần làm rõ

### Từ tài liệu cũ
- Đồng bộ lịch khám sang HIS là một chiều hay hai chiều? (nguồn: 10_open_questions.md)
- Nếu đồng bộ HIS thất bại thì lịch trên App/Web xử lý thế nào?
- Cần làm rõ RTO/RPO mục tiêu, cơ chế replication database và quy trình failover chi tiết (nguồn: 03_yeu_cau_tai_lieu_he_thong_mobile_web_his_2025_08_26.md)

### Từ tài liệu mới (20260619_luong_dong_bo_du_lieu_webapp_his.md)
1. **Chuẩn hóa tên**: "Thủy" và "Thủy" thì hệ thống bên ISOFH xác định là tên giống hay khác nhau?
2. **Thiếu CCCD**: Những trường hợp không có CCCD có đồng bộ được dữ liệu không? Đã test case này chưa?
3. **Đồng bộ các lần khám sau**: Dữ liệu đồng bộ như thế nào - kết thúc hồ sơ khám mới, hay mỗi dòng dữ liệu bất kỳ đều đẩy sang?
