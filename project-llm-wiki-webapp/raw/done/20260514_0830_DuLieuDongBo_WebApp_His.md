
# TRI THỨC VỀ ĐỒNG BỘ DỮ LIỆU GIỮA WEBAPP VÀ HIS

## 1. Dữ liệu đặt khám đẩy sang HIS gồm những thông tin gì ?
Dữ liệu đặt khám đẩy sang HIS gồm
Thông tin bệnh nhân
	• Họ tên: 
	• Giới tính: 
	• Ngày sinh:  
	• SĐT: 
	• CCCD/Giấy tờ:  
	• Loại giấy tờ: CCCD 
	• Địa chỉ chi tiết: 
Thông tin người giám hộ (bắt buộc vs NB dưới 14t)
	• Họ tên:  
	• SĐT: 
	• CCCD/Giấy tờ: 
Thông tin lịch khám
	• Mã tham chiếu: 
	• Lý do khám:  
	• Ngày đăng ký: 
Thông tin dịch vụ khám
	• Service ID:  
	• Mã dịch vụ: 
	• Phòng khám: 
	• Khoa: 
	• Giờ khám:  
	• Số thứ tự:
	
## 2. Trên màn hình HIS màn hình quản lý đặt khám ở đâu ?
Nhân viên y tế trên HIS sẽ xem ở màn hình "Danh sách NB đã tiếp đón", cột Người chỉ định (tên người chỉnh định vd: App VRR)

## 3. Ở chức năng tra cứu hồ sơ sức khoẻ : trường hợp nào thì dữ liệu từ HIS của người bệnh được đồng bộ sang web app ? (Hồ sơ lượt khám đã ở trạng thái hoàn tất hay cứ có thông tin hồ sơ lượt khám thì được phép đồng bộ luôn)
Khi có kết quả khám nào HIS sẽ gửi cho app kết quả đó (app chỉ hiển thị )

## 4. Thiết lập lịch làm việc : 

### 4.1. Khai báo cho từng ngày đúng không ? Trường hợp nếu file này sử dụng được cho nhiều ngày thì cột nào thể hiện được ngày làm việc của bác sĩ (ví dụ bác sĩ A làm vào 2,3,4, 7, CN, trong tuần) ? 
Khi thiết lập lịch phòng khám :
	- Trường hợp bác sĩ có các ngày làm việc khác nhau thì người dùng cần tạo từng ngày riêng biệt 
	- Trường hợp bác sĩ có cùng cấu hình và cùng thời gian: 
		• Chỉ cần nhập 1 lần 
		• Hệ thống hỗ trợ: 
			○ Lặp theo thứ (Thứ 2 → Thứ 6) 
			○ Hoặc lặp theo khoảng thời gian đó

### 4.2. Phần liên quan đến lịch làm việc có đồng bộ được phần nào từ HIS sang không ? 
Cần đồng bộ danh mục (khoa, chuyên khoa, phòng, dịch vụ, bác sĩ, loại đối tượng, loại hình thanh toán) từ HIS

### 4.3. Những thông tin nào bắt buộc phải có và những thông tin nào không bắt buộc ?
Chỉ có field Bác sĩ, Ghi chú là không bắt buộc, còn lại đều bắt buộc

### 4.4. File excel import lịch làm việc được dùng cho mục đích gì ?
- File Excel dùng để Viện điền thông tin lịch làm việc bác sĩ
- ISOFH sử dụng dữ liệu trong file để hỗ trợ Viện setup lịch làm việc trên màn hình “Lịch làm việc bác sĩ” để người bệnh đặt lịch khám
- File ban đầu thiếu cột “Ngày khám”, bổ sung thêm thông tin "Ngày khám". Ví dụ:
	+ Làm cố định: Thứ 2 đến Thứ 6
	+ hoặc Làm theo ngày cụ thể: Thứ 2, Thứ 3, Thứ 5,…
- Trường hợp thông tin lịch không thay đổi theo tuần (ví dụ: dịch vụ, khoa, phòng, chuyên khoa, khung giờ khám, thứ làm việc… giữ nguyên) thì bên tổng đài sẽ cấu hình: “Lặp lại hằng tuần đến” và chọn “ngày cụ thể” để hệ thống tự tạo lịch lặp theo tuần cho Viện.