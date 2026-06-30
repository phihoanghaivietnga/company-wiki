
# LUỒNG ĐỒNG BỘ DỮ LIỆU HỒ SƠ KHÁM SỨC KHỎE GIỮA WEBAPP VÀ HIS
Tài liệu giải thích luồng lấy dữ liệu hồ sơ khám sức khoẻ từ HIS về webapp để hiển thị thông tin trên webapp khi người bệnh tra cứu kết quả khám của mình

## 1. Câu hỏi từ Việt Nga
1. Hệ thống đang xác định những trường thông tin nào của bệnh nhân giữa HIS và Web để xác định đấy là 1 người bệnh để thực hiện đồng bộ dữ liệu ? Cùng 1 trường thông tin có phân biệt chữ hoa thường không ?
2. Cơ chế đồng bộ dữ liệu của người bệnh từ HIS về webapp như thế nào ? Do người quản trị  thực hiện thao tác đồng bộ ? Hay hệ thống thực hiện định kỳ đồng bộ ? Hay người bệnh cứ phát sinh thông tin thì đồng bộ luôn ?

## 2. Phản hồi của ISOFH
*1. Hệ thống đang xác định những trường thông tin nào của bệnh nhân giữa HIS và Web để xác định đấy là 1 người bệnh để thực hiện đồng bộ dữ liệu ? *
=> Để tra cứu Hồ sơ khám từ HIS hệ thống dựa vào: Họ tên, ngày tháng năm sinh, SĐT, Giới tính, CCCD or Mã NB
*Cùng 1 trường thông tin có phân biệt chữ hoa thường không ?*
=> Không. Riêng trường Họ và tên, hệ thống tự động chuẩn hóa về chữ in hoa trước khi thực hiện đối chiếu nên không bị ảnh hưởng bởi việc nhập chữ hoa hay chữ thường
*2. Cơ chế đồng bộ dữ liệu của người bệnh từ HIS về webapp như thế nào ? *
=> Để nhận được hồ sơ và kết quả khám từ HIS, người bệnh cần thực hiện tính năng Tra cứu kết quả khám trên App/Web
*Do người quản trị  thực hiện thao tác đồng bộ ? *
=>Không. Người bệnh tự thực hiện tra cứu trên App/Web. Vendor/Quản trị viên không có chức năng đồng bộ hồ sơ khám của người bệnh
*Hay hệ thống thực hiện định kỳ đồng bộ ? Hay người bệnh cứ phát sinh thông tin thì đồng bộ luôn ?*
=> Lần đầu tiên người bệnh cần thực hiện tra cứu để hệ thống liên kết và đồng bộ hồ sơ từ HIS.
Sau khi đã đồng bộ thành công một lần, các lần khám tiếp theo khi có kết quả mới trên HIS, hệ thống sẽ tự động trả kết quả về App/Web của người bệnh.

Cụ thể cơ chế đồng bộ là định kỳ 30 phút (hoặc giá trị cụ thể khác), hệ thống webapp sẽ lấy toàn bộ thông tin hồ sơ khám của người bệnh từ database của các cơ sở lưu vào cache của hệ thống webapp. Webapp sẽ lấy thông tin từ cache để hiển thị lên ứng dụng


## 3. Những câu hỏi cần tiếp tục làm rõ
1. "Thuỷ" và "Thủy" thì hệ thống bên ISOFH xác định là tên giống hay khác nhau ?
2. Khi thực hiện tra cứu kết quả khám lần đầu thì  tập hợp các giá trị [Họ tên, ngày tháng năm sinh, SĐT, Giới tính, CCCD và Mã NB (hiện tại đang bắt nhập Mã NB)] phải khớp nhau nhau giữa HIS và web app ? Hiện tại trên HIS số CCCD không phải là trường bắt buộc nhập thì những trường hợp thiếu CCCD không đồng bộ được dữ liệu đúng không ? Bên em đã test case này chưa ?
3. Các lần khám tiếp theo thì dữ liệu động bộ như thế nào : Kết thúc 1 hồ sơ khám mới đồng bộ ? Hay cứ phát sinh 1 dòng dữ liệu bất kỳ của người bệnh trong database trên HIS thì đẩy sang webapp ? Hay webapp không lưu dữ liệu hồ sơ khám của người bệnh mà thông tin hiển thị trên webapp được lấy trực tiếp từ HIS ?