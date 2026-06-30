# Đặt lịch khám trực tuyến

**Tóm tắt**: Chức năng cho phép người bệnh đặt lịch khám trực tuyến theo bác sĩ hoặc theo dịch vụ, với luồng xử lý bao gồm duyệt lịch bởi nhân viên y tế và đồng bộ sang HIS.

**Nguồn**: 04_plhd_app_03_03.md, 11_hdsd_web_tra_cuu.md, 20260612_0800_LuongDangKyKham.md

**Cập nhật lần cuối**: 13/06/2026

---

## Luồng đặt khám tổng thể

```
Người bệnh đặt lịch
    │
    ▼
Nhân viên y tế duyệt / từ chối lịch (trên Web Vendor)
    │
    ▼
00h hàng ngày → đồng bộ lịch hẹn sang HIS
```

(nguồn: 20260612_0800_LuongDangKyKham.md)

## Các bước đặt lịch khám

1. Tại trang chủ, chọn chức năng "Đặt lịch khám" (nguồn: 11_hdsd_web_tra_cuu.md)
2. Chọn tab "Đặt lịch khám theo bác sĩ" hoặc "Đặt lịch khám theo dịch vụ"
3. Nhập thông tin tìm kiếm (tên NB, triệu chứng, khoa/chuyên khoa)
4. Chọn hồ sơ người bệnh và nhấn "Đặt khám"
5. Chọn dịch vụ/bác sĩ phù hợp, xem lịch khả dụng
6. Chọn ngày và khung giờ khám khả dụng
7. Kiểm tra toàn bộ thông tin tại màn hình xác nhận, nhấn "Đặt lịch hẹn"
8. Kiểm tra thông báo "Đặt lịch hẹn thành công"

## Ràng buộc slot khám

- Không cho phép đặt lịch vào các khung giờ đã có bệnh nhân đặt lịch đủ (nguồn: 04_plhd_app_03_03.md)
- Hệ thống cần có cơ chế kiểm tra số lượng đặt khám theo slot
- Có phân chia buổi sáng/chiều theo khung giờ

## Thiết lập lịch làm việc bác sĩ

- Khi thiết lập lịch phòng khám, nếu bác sĩ có các ngày làm việc khác nhau thì cần tạo từng ngày riêng biệt (nguồn: 20260514_0830_DuLieuDongBo_WebApp_His.md)
- Nếu bác sĩ có cùng cấu hình và cùng thời gian: chỉ cần nhập 1 lần, hỗ trợ lặp theo thứ (Thứ 2 → Thứ 6) hoặc lặp theo khoảng thời gian
- Chỉ có field Bác sĩ, Ghi chú là không bắt buộc, còn lại đều bắt buộc
- Cần đồng bộ danh mục (khoa, chuyên khoa, phòng, dịch vụ, bác sĩ, loại đối tượng, loại hình thanh toán) từ HIS

### File Excel import lịch làm việc

- File Excel dùng để Viện điền thông tin lịch làm việc bác sĩ (nguồn: 20260514_0830_DuLieuDongBo_WebApp_His.md)
- ISOFH sử dụng dữ liệu trong file để hỗ trợ Viện setup lịch làm việc
- File cần có cột "Ngày khám"
- Trường hợp lịch không thay đổi theo tuần, tổng đài sẽ cấu hình "Lặp lại hằng tuần đến" để hệ thống tự tạo lịch

## Duyệt lịch và thông báo

- Nhân viên y tế duyệt/từ chối lịch trên Web Vendor (nguồn: 20260612_0800_LuongDangKyKham.md)
- Lịch hẹn chỉ ghi nhận khi có thông báo đặt lịch thành công (nguồn: 11_hdsd_web_tra_cuu.md)
- Số thứ tự dự kiến sẽ được gửi lại sau khi tổng đài viên duyệt lịch trong ngày
- Vào 00h hàng ngày đồng bộ lịch hẹn đã duyệt sang HIS

## Đặt khám cho người thân

- Hỗ trợ quản lý hồ sơ người thân (nguồn: 04_plhd_app_03_03.md)
- Hỗ trợ đặt khám cho người thân
- Hỗ trợ xem kết quả hoặc hồ sơ liên quan người thân

## Các vấn đề cần làm rõ

- Slot khám được tính theo bác sĩ, dịch vụ, phòng, chuyên khoa hay tổ hợp? (nguồn: 10_open_questions.md)
- Quy tắc "đã đủ bệnh nhân" lấy từ đâu: cấu hình slot, lịch HIS, hay Web Vendor?
- Có cho phép hủy/đổi lịch không?
- Có yêu cầu thanh toán trước khi xác nhận lịch không?

## Các trang liên quan

- [[dong-bo-du-lieu-his]]
- [[luong-dang-ky-kham]]
- [[web-vendor]]
- [[ho-so-suc-khoe]]