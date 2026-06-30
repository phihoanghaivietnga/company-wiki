
## Luồng hệ thống

Khách hàng đến lấy số -> Lễ tân chỉ định dịch vụ khám dựa trên yêu cầu của khách hàng (Module Quản lý tiếp đón) 
	-> Thu ngân thu tiền & in phiếu (Module Thu ngân) -> Quay về Lễ tân chỉ định (Module Quản lý tiếp đón) 
	->  (Đo thị lực + Chẩn đoán hình ảnh) để lấy kết quả 
	-> Vào phòng khám (Khám bệnh)
		-> Kết luận -> chỉ định thuốc (Module Nhà thuốc / Quầy kính) -> khách hàng đi về
		-> chỉ định nội trú (Module Quản lý nội trú) -> Xét nghiệm (Module LIS) -> Kết luận xét nghiệm -> Đủ điều kiện được phẫu thuật (Module Phẫu thuật Thủ thuật) -> chỉ định thuốc (Module Nhà thuốc / Quầy kính) -> khách hàng đi về