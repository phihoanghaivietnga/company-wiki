# Partner Tracker — ISOFH (WebApp BV Mắt Quốc tế Việt Nga)

**Cập nhật lần cuối**: 2026-07-01 (snapshot: issue_webapp_20260701.xlsx)

---

## Đang mở (open)

### [ISSUE-001] Thêm lịch làm việc của bác sĩ — Không cho lập ngày làm việc vào Chủ nhật & không lập được lịch cho ngày đã qua
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Thêm lịch làm việc của bác sĩ
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Ngày báo cáo**: 2026-05-14
- **Người báo cáo**: HaiPH
- **Mô tả**: Lịch làm việc chỉ áp dụng khai báo cho từng tuần và không cho lập ngày làm việc vào Chủ nhật. Đang thứ 5 không thể lập lịch làm việc cho thứ 2,3,4.
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi lại Việt Nga (kèm ảnh: https://prnt.sc/KpzwvJ10In0x)
- **Ý kiến của VN**: (chưa có)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-05-14
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]]

### [ISSUE-002] Thêm lịch làm việc của bác sĩ — Combobox Dịch vụ chỉ cho phép chọn 1 dịch vụ
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Thêm lịch làm việc của bác sĩ
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: 1 bác sĩ có thể phụ trách nhiều dịch vụ nhưng combobox Dịch vụ chỉ cho phép chọn 1 dịch vụ.
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi lại Việt Nga (kèm ảnh: https://prnt.sc/EPwhMvsYegTy)
- **Ý kiến của VN**: (chưa có)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]]

### [ISSUE-003] Quản lý tin tức — Đã thêm mới bài viết nhưng không hiển thị ở danh sách
- **Loại**: Lỗi
- **Chức năng**: Quản lý tin tức
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Đã thêm mới bài viết nhưng ở ngoài màn hình danh sách bài viết không thấy hiển thị.
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi lại Việt Nga (kèm ảnh: https://prnt.sc/WTz0OxfGNsC8)
- **Ý kiến của VN**: (chưa có)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[thong-bao-tin-tuc]], [[web-vendor]]

### [ISSUE-004] Quản lý thông báo — Không thấy chức năng tạo danh mục Tên màn hình, Màn hình chi tiết
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Quản lý thông báo → Tin thông báo
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Không thấy chức năng tạo thông tin danh mục Tên màn hình, Màn hình chi tiết.
- **Ý kiến đối tác (ISOFH)**: Tính năng gửi thông báo tùy chỉnh bởi admin không có trong hợp đồng, bên em sẽ ẩn đi ạ.
- **Ý kiến của VN**: (chưa có)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[thong-bao-tin-tuc]], [[web-vendor]]

### [ISSUE-005] Sửa thông tin người bệnh — Không cập nhật được + Tìm theo SĐT không ra
- **Loại**: Lỗi
- **Chức năng**: Sửa thông tin người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Ngày báo cáo**: 2026-06-13
- **Người báo cáo**: HaiPH
- **Mô tả**: 
  1. Ở menu danh sách tài khoản, chọn tài khoản để sửa (tìm kiếm theo SĐT) → chọn sửa → nhập thông tin → khi lưu báo lỗi "Cập nhật thông tin người bệnh thất bại" → không biết nguyên nhân cụ thể.
  2. Khi tìm kiếm thông tin người bệnh ở màn hình Đặt lịch khám thì tìm theo SĐT không ra được.
- **Ý kiến đối tác (ISOFH)**: 
  1. Đối với các trường hợp cần cập nhật thông tin người bệnh, đề nghị thực hiện trên Web Kết quả thông qua chức năng Sửa thông tin người bệnh. Vendor hiện không hỗ trợ chỉnh sửa đầy đủ hồ sơ người bệnh. Một số trường hợp phát sinh lỗi do dữ liệu người bệnh được tạo từ các phiên bản trước đang thiếu medicalProfileId.
  2. Chức năng "Đặt lịch khám" mới trên màn hình Vendor/Tổng đài hiện chưa được triển khai. Hỏi: Viện có nhân viên trực tổng đài và nhận cuộc gọi đặt khám qua Tổng đài không?
- **Ý kiến của VN**: 
  1. Những trường hợp dữ liệu lỗi do được tạo từ phiên bản trước thì hướng xử lý của ISOFH như thế nào? Cần làm rõ web vendor được sửa những thông tin nào của người bệnh?
  2. Viện có nhân viên CSKH nhận cuộc gọi và đặt khám qua tổng đài (vẫn đang thực hiện).
- **Trạng thái**: Đang trao đổi
- **Lần đầu xuất hiện**: 2026-06-13
- **Lần cập nhật gần nhất**: 2026-07-01
- **Liên kết**: [[web-vendor]], [[dong-bo-du-lieu-his]], [[xac-thuc-bao-mat]]

### [ISSUE-006] Tại sao phải nhập lại thông tin người bệnh khi đồng bộ từ HIS? Không đồng bộ tự động?
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Đồng bộ thông tin bệnh nhân HIS → WebApp
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Ngày báo cáo**: 2026-06-15
- **Người báo cáo**: HaiPH
- **Mô tả**: 
  - Tại sao khi đồng bộ thông tin bệnh nhân từ HIS về webapp phải nhập lại đúng thông tin người bệnh đã cung cấp ở bệnh viện?
  - Nếu nhập thông tin sai khác thì sao?
  - Tại sao không thực hiện đồng bộ thông tin người bệnh từ HIS về?
- **Ý kiến đối tác (ISOFH)**: Hệ thống được thiết kế theo nguyên tắc xác thực và bảo mật: NB cần đăng ký tài khoản → khai báo/cập nhật thông tin → tra cứu bằng CCCD hoặc Mã NB. Hệ thống đối soát thông tin với HIS và chỉ trả kết quả khi khớp hoàn toàn. Sử dụng SĐT làm điều kiện tra cứu hoặc tự động đồng bộ theo SĐT tiềm ẩn rủi ro bảo mật dữ liệu y tế (1 SĐT dùng cho nhiều NB, người thân đăng ký hộ...).
- **Ý kiến của VN**: 
  1. Cần làm rõ hệ thống đối soát giữa HIS và WebApp như thế nào, gồm những trường thông tin nào? → ISOFH: Khi đăng ký, HIS không đối soát gì. Chỉ khi tra cứu bằng CCCD hoặc Mã NB, App mới đối soát thông tin NB trên HIS và App có match không. Hệ thống tự format chữ in hoa họ tên. Dấu THỦY/THUỶ do bộ gõ khác nhau HIS hiểu là khác, nhưng khi checkin HIS không check dấu.
  2. Nếu SĐT tiềm ẩn rủi ro bảo mật, sao lại cho phép dùng SĐT làm ID đăng nhập? → ISOFH: SĐT chỉ dùng để đăng nhập/xác thực cơ bản và hỗ trợ liên hệ, không dùng để pull toàn bộ dữ liệu khám. Điều kiện tra cứu hồ sơ khám bắt buộc qua CCCD hoặc Mã NB, sau đó đối soát họ tên, ngày sinh, giới tính và SĐT.
- **Trạng thái**: Đang trao đổi
- **Lần đầu xuất hiện**: 2026-06-15
- **Lần cập nhật gần nhất**: 2026-07-01
- **Liên kết**: [[dong-bo-du-lieu-his]], [[xac-thuc-bao-mat]], [[ho-so-suc-khoe]]

### [ISSUE-007] Tra cứu hồ sơ sức khỏe bắt buộc nhập Mã bệnh nhân — Người dùng không nhớ được
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Tra cứu hồ sơ sức khỏe
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Ngày báo cáo**: 2026-06-15
- **Người báo cáo**: HaiPH
- **Mô tả**: Khi tra cứu thông tin hồ sơ sức khoẻ, hệ thống bắt người dùng phải nhập Mã bệnh nhân nhưng người dùng không nhớ được thì phải làm thế nào?
- **Ý kiến đối tác (ISOFH)**: NB có thể tra cứu bằng CCCD, ISOFH sẽ triển khai thêm tính năng cho Web trả kết quả.
- **Trạng thái**: Hoàn thành PROD
- **Lần đầu xuất hiện**: 2026-06-15
- **Liên kết**: [[ho-so-suc-khoe]]

### [ISSUE-008] Tài khoản người bệnh bị khoá — Làm thế nào để mở lại?
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Quản lý tài khoản người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Ngày báo cáo**: 2026-06-15
- **Người báo cáo**: HaiPH
- **Mô tả**: Tài khoản người bệnh bị khoá, phải làm thế nào để mở lại cho người bệnh sử dụng được?
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi lại Việt Nga (kèm ảnh: https://prnt.sc/xQqUUVTf5TNk)
- **Ý kiến của VN**: Đồng ý
- **Trạng thái**: Đã có hướng dẫn từ ISOFH, VN đồng ý
- **Lần đầu xuất hiện**: 2026-06-15
- **Lần cập nhật gần nhất**: 2026-07-01
- **Liên kết**: [[xac-thuc-bao-mat]], [[web-vendor]]

### [ISSUE-009] Thông tin chi tiết người bệnh trên Vendor không hiển thị đầy đủ
- **Loại**: Lỗi
- **Chức năng**: Quản lý thông tin người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Người bệnh đã đăng ký mới thông tin trên web tra cứu (đầy đủ các thông tin hành chính), nhưng khi quản trị viên vào màn hình thông tin chi tiết của người bệnh thì chỉ duy nhất số điện thoại hiển thị, toàn bộ các thông tin còn lại đều không hiển thị.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[web-vendor]], [[dong-bo-du-lieu-his]]

### [ISSUE-010] HIS không bắt buộc nhập CCCD nhưng WebApp bắt buộc nhập CCCD
- **Loại**: Vướng mắc / Conflict
- **Chức năng**: Đăng ký tài khoản người bệnh
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Trên HIS hiện tại không bắt buộc nhập số CCCD, tại sao trên web tra cứu lại bắt buộc nhập số CCCD? Trường hợp nếu trên HIS không có số CCCD thì người bệnh đăng ký kiểu gì?
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi (kèm ảnh: https://prnt.sc/_FoGEUf9uEyr)
- **Ý kiến của VN**: 
  1. Vấn đề thống nhất cách xử lý dữ liệu giữa HIS và WebApp: HIS KHÔNG bắt buộc nhập CCCD, WebApp bắt buộc nhập CCCD → thông tin không bao giờ khớp với những NB không để lại CCCD. WebApp căn cứ vào đâu để bắt buộc nhập CCCD?
  2. Những trường hợp khi đi khám không để lại số CCCD thì NB làm thế nào để đăng ký trên ứng dụng tra cứu sức khoẻ?
- **Trạng thái**: Đang trao đổi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dong-bo-du-lieu-his]], [[xac-thuc-bao-mat]], [[ho-so-suc-khoe]]

⚠️ **CONFLICT TIỀM ẨN**: Yêu cầu bắt buộc CCCD trên WebApp mâu thuẫn với thực tế HIS không bắt buộc CCCD. Cần thống nhất phương án xử lý cho NB không có CCCD trên HIS.

### [ISSUE-011] Làm rõ luồng đồng bộ thông tin người bệnh mới từ HIS về WebApp
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Đồng bộ dữ liệu HIS ↔ WebApp
- **Nền tảng**: Web tra cứu / App
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: 
  1. Hệ thống đang xác định những trường thông tin nào của bệnh nhân giữa HIS và Web để xác định đấy là 1 người bệnh để thực hiện đồng bộ dữ liệu? Cùng 1 trường thông tin có phân biệt chữ hoa thường không?
  2. Cơ chế đồng bộ dữ liệu của người bệnh từ HIS về webapp như thế nào? Do người quản trị thực hiện thao tác đồng bộ? Hay hệ thống thực hiện định kỳ đồng bộ? Hay người bệnh cứ phát sinh thông tin thì đồng bộ luôn?
- **Trạng thái**: Chờ ISOFH phản hồi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dong-bo-du-lieu-his]]

### [ISSUE-012] App — Gõ nội dung đặt câu hỏi cho bác sĩ không nhìn thấy chữ
- **Loại**: Lỗi
- **Chức năng**: Giải đáp thắc mắc (đặt câu hỏi cho bác sĩ)
- **Nền tảng**: App
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Khi gõ nội dung đặt câu hỏi cho bác sĩ thì không nhìn thấy gì, phải bôi đen thì mới nhìn được nội dung.
- **Ý kiến đối tác (ISOFH)**: Bên em sẽ tiến hành chỉnh sửa lại phần này ạ.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[ho-so-suc-khoe]]

### [ISSUE-013] Sửa thông tin người bệnh — Ràng buộc Tỉnh/TP, Phường/Xã theo Quốc tịch không chính xác
- **Loại**: Lỗi
- **Chức năng**: Sửa thông tin người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở phần sửa thông tin người bệnh, phần thông tin Tỉnh/thành phố, Phường/Xã đang ràng buộc theo Quốc tịch. Phần này không chính xác vì quốc tịch và nơi ở hiện tại không liên quan đến nhau (người nước ngoài đang sống tại Việt Nam).
- **Trạng thái**: Chờ xử lý
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[web-vendor]]

### [ISSUE-014] Sửa thông tin người bệnh — Trường Ngày cấp hiển thị trắng sau khi lưu
- **Loại**: Lỗi
- **Chức năng**: Sửa thông tin người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: UAT
- **Người báo cáo**: HaiPH
- **Mô tả**: Sau khi sửa thông tin người bệnh, đã ấn nút lưu nhưng chọn lại thì trường Ngày cấp vẫn hiển thị giá trị trắng.
- **Ý kiến đối tác (ISOFH)**: Bên em sẽ fix lại mục này ạ.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[web-vendor]]

### [ISSUE-015] Danh sách người bệnh trên Vendor không hiển thị SĐT
- **Loại**: Lỗi
- **Chức năng**: Danh sách người bệnh
- **Nền tảng**: Web Vendor
- **Môi trường**: UAT
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở màn hình danh sách người bệnh trên web vendor, mặc dù người bệnh có số điện thoại nhưng không hiển thị ra màn hình (https://prnt.sc/0xCWxRrw9K9x). Khi vào màn hình sửa thông tin thì vẫn hiển thị đúng số điện thoại.
- **Ý kiến đối tác (ISOFH)**: UAT Web tra cứu đang gọi API http://api-uat.vrr.vn, ISOFH sửa lại uat-api-ketqua.vrr.vn. Nhờ Viện kiểm tra lại sau khi ISOFH hoàn thành.
- **Trạng thái**: Chờ VN kiểm tra lại
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[web-vendor]]

### [ISSUE-016] Người bệnh có trên Vendor nhưng đăng nhập Web tra cứu báo lỗi
- **Loại**: Lỗi
- **Chức năng**: Đăng nhập Web tra cứu
- **Nền tảng**: Web tra cứu
- **Môi trường**: UAT
- **Người báo cáo**: HaiPH
- **Mô tả**: Người bệnh đã có thông tin trên web vendor (https://prnt.sc/cVYgVmnZn_0H), ở trạng thái không khoá nhưng khi đăng nhập trên web tra cứu kết quả thì hệ thống báo lỗi (https://prnt.sc/XewX_CCsKRkL).
- **Ý kiến đối tác (ISOFH)**: (kèm link API: http://uat-api-ketqua.vrr.vn/)
- **Trạng thái**: Chờ xử lý
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[xac-thuc-bao-mat]], [[web-vendor]]

### [ISSUE-017] Đăng ký trên Web tra cứu UAT báo lỗi "Đăng ký thất bại" không rõ nguyên nhân
- **Loại**: Lỗi
- **Chức năng**: Đăng ký tài khoản
- **Nền tảng**: Web tra cứu
- **Môi trường**: UAT
- **Người báo cáo**: HaiPH
- **Mô tả**: Đã tạo thông tin người bệnh trên UAT nhưng khi thực hiện đăng ký trên web tra cứu thì bị thông báo lỗi "Đăng ký thất bại" mà không biết nguyên nhân do đâu.
- **Ý kiến đối tác (ISOFH)**: UAT Web tra cứu đang gọi API http://api-uat.vrr.vn, ISOFH sửa lại uat-api-ketqua.vrr.vn. Nhờ Viện kiểm tra lại sau khi ISOFH hoàn thành.
- **Trạng thái**: Chờ VN kiểm tra lại
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[xac-thuc-bao-mat]]

### [ISSUE-018] Danh sách bác sĩ — Filter Chuyên khoa "Mắt" không lọc được
- **Loại**: Lỗi
- **Chức năng**: Danh sách bác sĩ
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở màn hình danh sách bác sĩ, ô textbox filter "Chuyên khoa" chọn "Mắt" nhập thông tin chuyên khoa để lọc nhưng không thấy danh sách bác sĩ lọc theo filter.
- **Ý kiến đối tác (ISOFH)**: Bên em sẽ fix lại mục này ạ.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]]

### [ISSUE-019] Đề nghị sửa giao diện tra cứu giống Đại học Y — Chọn tìm theo CCCD hoặc Mã BN
- **Loại**: Feature Request
- **Chức năng**: Tra cứu hồ sơ sức khỏe
- **Nền tảng**: Web tra cứu
- **Môi trường**: UAT
- **Người báo cáo**: HaiPH
- **Mô tả**: Đề nghị sửa lại giao diện và cách tra cứu giống như Đại học Y, người dùng có thể lựa chọn tìm theo số CCCD hoặc theo mã bệnh nhân. Số CCCD mặc định lấy từ database người dùng khai báo lần đầu, nếu có nhiều mã bệnh nhân thì hiển thị danh sách để người dùng chọn.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[ho-so-suc-khoe]]

### [ISSUE-020] Duyệt lịch đặt khám — Combo Loại đối tượng & Loại thanh toán không hiển thị giá trị
- **Loại**: Lỗi
- **Chức năng**: Duyệt lịch đặt khám
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở chức năng duyệt lịch đặt khám, combo Loại đối tượng người bệnh và loại thanh toán không hiển thị danh sách các giá trị để chọn nên không thể duyệt được.
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi (kèm ảnh: https://prnt.sc/krCJutI-o28Z)
- **Ý kiến của VN**: Trong màn hình danh sách loại đối tượng và loại hình thanh toán chỉ có nút "Đồng bộ" chứ không có icon sửa thông tin. Ngoài ra cả 2 danh mục này đều có danh sách giá trị rồi nhưng không hiển thị trên màn hình duyệt.
- **Trạng thái**: Đang trao đổi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]], [[luong-dang-ky-kham]]

### [ISSUE-021] Từ chối lịch đặt khám — Không gửi được tin nhắn Zalo
- **Loại**: Lỗi
- **Chức năng**: Từ chối lịch đặt khám + Gửi Zalo
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở chức năng từ chối lịch đặt khám, log thông báo không gửi được tin nhắn trả lời về cho khách hàng. Không gửi được tin nhắn Zalo.
- **Ý kiến của VN**: Đã nhận được tin nhắn từ đặt lịch hẹn.
- **Trạng thái**: Hoàn thành PROD
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[zalo-sms]], [[dat-lich-kham]], [[luong-dang-ky-kham]]

### [ISSUE-022] Đặt lịch khám theo dịch vụ — Combo lọc theo dịch vụ không hiển thị giá trị
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Đặt lịch khám theo dịch vụ
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Ở chức năng đặt lịch khám theo dịch vụ, combo lọc theo dịch vụ không hiển thị danh sách các giá trị mặc dù trong web vendor đã có các giá trị dịch vụ.
- **Ý kiến đối tác (ISOFH)**: Chỉ các dịch vụ khám đã được cấu hình "Lịch làm việc" trên Vendor mới được phép hiển thị trên màn hình web tra cứu. Các dịch vụ khám không có lịch làm việc / không triển khai đặt khám thì sẽ không hiển thị. Nếu hiển thị toàn bộ sẽ dẫn đến dư thừa dữ liệu, người dùng chọn được dịch vụ không thể đặt khám.
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]]

### [ISSUE-023] Báo cáo thống kê — Bị báo lỗi không rõ nguyên nhân
- **Loại**: Lỗi
- **Chức năng**: Báo cáo thống kê
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Vào chức năng báo cáo thống kê bị báo lỗi (không rõ nguyên nhân cụ thể).
- **Ý kiến đối tác (ISOFH)**: Bên em sẽ fix lại mục này ạ.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[web-vendor]]

### [ISSUE-024] Màn hình chờ xác thực — Nội dung dễ gây nhầm lẫn SMS vs Zalo
- **Loại**: Feature Request / UX
- **Chức năng**: Xác thực OTP
- **Nền tảng**: Web tra cứu / App
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Hiện tại ở màn hình chờ xác thực đang hiển thị nội dung "Vui lòng nhập mã xác thực được gửi đến số điện thoại ****" → dễ gây nhầm lẫn cho NB là tin nhắn SMS của nhà mạng (không mở tin nhắn Zalo để lấy số xác thực). Sửa lại: "Vui lòng nhập mã xác thực được gửi đến Zalo của số điện thoại ****".
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[xac-thuc-bao-mat]], [[zalo-sms]]

### [ISSUE-025] Không gửi được tin nhắn Zalo xác nhận lịch hẹn (lỗi template)
- **Loại**: Lỗi
- **Chức năng**: Gửi Zalo xác nhận lịch hẹn
- **Nền tảng**: Web Vendor
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Không gửi được tin nhắn Zalo xác nhận lịch hẹn (báo lỗi template data is missing a parameter dich_vu).
- **Trạng thái**: Hoàn thành PROD
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[zalo-sms]], [[dat-lich-kham]]

### [ISSUE-026] App — Đặt lịch khám cho con dưới 14 tuổi báo lỗi chưa có thông tin người bảo lãnh
- **Loại**: Lỗi
- **Chức năng**: Đặt lịch khám
- **Nền tảng**: App
- **Môi trường**: PROD
- **Người báo cáo**: HaiPH
- **Mô tả**: Khi đặt lịch khám cho con (dưới 14 tuổi), mặc dù đã có đầy đủ thông tin người bảo lãnh nhưng hệ thống vẫn báo lỗi chưa có thông tin người bảo lãnh → không thể đặt lịch hẹn khám được.
- **Trạng thái**: Đang thực hiện
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[luong-dang-ky-kham]]

### [ISSUE-027] Đổi mật khẩu — Không có chức năng đổi mật khẩu trên Web tra cứu
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Quản lý tài khoản
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: Huyền
- **Mô tả**: Tài khoản đã tạo để tra cứu kết quả khám, đăng nhập không vào được. Reset về mật khẩu mặc định 123456 mới đăng nhập được. Đăng nhập thành công, giờ người dùng muốn đổi mật khẩu thì thao tác ở đâu?
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi (kèm ảnh: https://prnt.sc/OTecAqu5Vu_e)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[xac-thuc-bao-mat]]

### [ISSUE-028] Chính sách khóa tài khoản khi nhập sai mật khẩu
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Xác thực đăng nhập
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Nhập sai mật khẩu tối đa bao nhiêu lần thì tạm khóa không cho đăng nhập? Sau bao lâu thì thử lại?
- **Ý kiến đối tác (ISOFH)**: Hệ thống cho phép người dùng nhập sai mật khẩu tối đa 5 lần liên tiếp. Nếu vượt quá số lần quy định thì sẽ không thể đăng nhập. Cần chờ sau 5 phút để thực hiện đăng nhập lại.
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[xac-thuc-bao-mat]]

### [ISSUE-029] Web tra cứu PROD — Lỗi (ảnh)
- **Loại**: Lỗi
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Mô tả**: (chỉ có ảnh: https://prnt.sc/lLb5SL5OdxoR — cần mô tả chi tiết)
- **Trạng thái**: Cần làm rõ
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: (chưa xác định)

⚠️ **DỮ LIỆU MƠ HỒ**: Issue này chỉ có link ảnh, không có mô tả bằng văn bản. Cần bổ sung mô tả chi tiết.

### [ISSUE-030] Web tra cứu PROD — Lỗi (ảnh)
- **Loại**: Lỗi
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Mô tả**: (chỉ có ảnh: https://prnt.sc/-LTJgNxTF5A6. — cần mô tả chi tiết)
- **Trạng thái**: Cần làm rõ
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: (chưa xác định)

⚠️ **DỮ LIỆU MƠ HỒ**: Issue này chỉ có link ảnh, không có mô tả bằng văn bản. Cần bổ sung mô tả chi tiết.

### [ISSUE-031] Web tra cứu PROD — Giao diện danh sách bác sĩ (so sánh với ĐHY)
- **Loại**: Feature Request / UX
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: (kèm ảnh: https://prnt.sc/nJEOr2PCccI5) — Liên quan đến giao diện danh sách bác sĩ.
- **Ý kiến đối tác (ISOFH)**: Em đã kiểm tra hình ảnh thì thấy giao diện của ĐHY, anh/chị muốn xây dựng theo ĐHY phải không ạ?
- **Ý kiến của VN**: List bác sĩ trên giao diện hiện tại, em có để lại ảnh rồi ạ.
- **Trạng thái**: Đang trao đổi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]]

### [ISSUE-032] Web tra cứu PROD — Lỗi (ảnh)
- **Loại**: Lỗi
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Mô tả**: (chỉ có ảnh: https://prnt.sc/zmmA3xSpoyau — cần mô tả chi tiết)
- **Trạng thái**: Cần làm rõ
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: (chưa xác định)

⚠️ **DỮ LIỆU MƠ HỒ**: Issue này chỉ có link ảnh, không có mô tả bằng văn bản. Cần bổ sung mô tả chi tiết.

### [ISSUE-033] Hủy đặt lịch — Thời gian hủy, tự động duyệt, phân biệt BHYT
- **Loại**: Vướng mắc / Câu hỏi
- **Chức năng**: Đặt lịch khám
- **Nền tảng**: Web tra cứu
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Hủy đặt lịch trước bao lâu, sau bao lâu thì hệ thống tự ghi nhận đặt lịch thành công? Hiện đang thấy phải có người duyệt bên vendor. Duyệt lịch sao biết được đối tượng NB là theo yêu cầu hay có khám BHYT?
- **Trạng thái**: Chờ phản hồi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[luong-dang-ky-kham]]

### [ISSUE-034] Tìm theo tên dịch vụ — Cần hiển thị danh sách dịch vụ
- **Loại**: Feature Request / UX
- **Chức năng**: Đặt lịch khám theo dịch vụ
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Tìm theo tên dịch vụ, yêu cầu hiển thị danh sách dịch vụ, người dùng không biết để nhập được.
- **Trạng thái**: Chờ phản hồi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]]

### [ISSUE-035] Tách riêng Đặt lịch theo bác sĩ và Đặt lịch theo dịch vụ?
- **Loại**: Feature Request / UX
- **Chức năng**: Đặt lịch khám
- **Nền tảng**: Web tra cứu
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Có cần tách riêng Đặt lịch khám theo bác sĩ và Đặt lịch khám theo dịch vụ? Đa số NB không biết dịch vụ là gì để mà chọn. Thường đặt khám theo bác sĩ hoặc theo chuyên khoa. Đặt lịch khám theo dịch vụ hiện đang lấy theo lịch khám của bác sĩ, có bác sĩ khám mới có dịch vụ.
- **Trạng thái**: Chờ phản hồi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]]

### [ISSUE-036] Đặt lịch khám chỉ hiển thị 1 bác sĩ của ngày hiện tại
- **Loại**: Lỗi / Vướng mắc
- **Chức năng**: Đặt lịch khám
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Vendor khai báo lịch làm việc của các bác sĩ, đặt lịch khám chỉ đang hiển thị 1 bác sĩ của ngày hiện tại. Yêu cầu: Hiển thị tất cả các BS có lịch làm việc được khai báo trong tuần.
- **Ý kiến đối tác (ISOFH)**: ISOFH đã phản hồi (kèm ảnh: https://prnt.sc/eHy_q2_j8q2A)
- **Trạng thái**: Chờ phản hồi từ VN
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: [[dat-lich-kham]], [[web-vendor]]

### [ISSUE-037] Bổ sung mã QR trên giao diện web để tải app
- **Loại**: Feature Request
- **Chức năng**: Giao diện Web tra cứu
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Người báo cáo**: (chưa rõ)
- **Mô tả**: Bổ sung thêm phần mã QR trên giao diện web để người dùng quét tải app về điện thoại.
- **Trạng thái**: Chờ phản hồi
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: (chưa xác định)

### [ISSUE-038] Web tra cứu PROD — Lỗi (ảnh)
- **Loại**: Lỗi
- **Nền tảng**: Web tra cứu
- **Môi trường**: PROD
- **Mô tả**: (chỉ có ảnh: https://prnt.sc/i-p52YoZMPZP — cần mô tả chi tiết)
- **Trạng thái**: Cần làm rõ
- **Lần đầu xuất hiện**: 2026-07-01 (sync lần đầu)
- **Liên kết**: (chưa xác định)

⚠️ **DỮ LIỆU MƠ HỒ**: Issue này chỉ có link ảnh, không có mô tả bằng văn bản. Cần bổ sung mô tả chi tiết.

---

## Đã đóng (resolved)

### [ISSUE-007] Tra cứu hồ sơ sức khỏe — Bắt buộc nhập Mã BN, NB không nhớ được
- **Trạng thái cuối**: Hoàn thành PROD
- **Tóm tắt**: ISOFH xác nhận NB có thể tra cứu bằng CCCD, sẽ triển khai thêm tính năng cho Web trả kết quả.

### [ISSUE-021] Từ chối lịch đặt khám — Không gửi được tin nhắn Zalo
- **Trạng thái cuối**: Hoàn thành PROD
- **Tóm tắt**: VN xác nhận đã nhận được tin nhắn từ đặt lịch hẹn.

### [ISSUE-025] Không gửi được tin nhắn Zalo xác nhận lịch hẹn (lỗi template dich_vu)
- **Trạng thái cuối**: Hoàn thành PROD
- **Tóm tắt**: Đã fix lỗi template Zalo.

---

## Tham chiếu — Kế hoạch triển khai (Plan 05)

Các tính năng đã hoàn thành theo kế hoạch Plan 05 (tham khảo từ sheet "Plan 05"):

| # | Loại | Hệ thống | Chức năng | Môi trường | Trạng thái |
|---|---|---|---|---|---|
| 01 | Feature | App / Web NB | Tích hợp ZNS OTP (xác thực đăng ký / quên mật khẩu) | PROD | Completed |
| 02 | Feature | App / Web NB | Đăng ký / Đăng nhập bằng SĐT/OTP | PROD | Completed |
| 03 | Feature | App / Web NB | Quản lý thông tin NB / Thành viên | PROD | Completed |
| 04 | Feature | App NB | Giải đáp thắc mắc (đặt câu hỏi, upload ảnh, gọi hotline) | PROD | Completed |
| 05 | Feature | App NB | Thông báo đẩy (Push) & Tin tức y tế | PROD | Completed |
| 06 | Task | App/Web NB, Vendor | Build UAT để viện test | UAT | Completed |
| 07 | Feature | Web Vendor, HIS | Đồng bộ danh mục & danh sách bác sĩ từ HIS | PROD | Completed |
| 08 | Feature | Web/App NB, HIS | Tra cứu kết quả khám (bổ sung chọn cơ sở) | PROD | Completed |
| 09 | Feature | Web/App NB, Vendor, HIS | Xem kết quả khám theo cơ sở | PROD | Completed |
| 10 | Feature | Web/App NB, Vendor, HIS | Đặt lịch khám (đầy đủ luồng) | PROD | Completed |
| 11 | Feature | App / Web NB | Tích hợp ZNS (duyệt lịch, từ chối lịch) | PROD | Completed |

---

## Nhân sự ISOFH (tham khảo)

| Role | Tên |
|---|---|
| PM | Hoàng Văn Trưởng |
| BA | Lê Thị Hoàng Kiều |
| QC | Phạm Trà My |
| System | Nguyễn Hữu Linh |
| BE | Đỗ Duy Mậu |
| FE (Web Vendor) | Lê Anh Tú |
| FE (App người NB) | Mai Đức Anh |
| FE (Web người NB) | Nguyễn Ngọc Dự |