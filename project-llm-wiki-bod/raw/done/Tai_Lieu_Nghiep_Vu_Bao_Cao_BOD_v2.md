# **TÀI LIỆU QUY TẮC NGHIỆP VỤ BÁO CÁO**

## **Phân hệ: Phân Tích Giữ Chân Khách Hàng & Quản Trị Tái Khám**

**Ngày lập:** 25/05/2026

## **1\. MỤC TIÊU CỦA BÁO CÁO**

Màn hình báo cáo này được thiết kế nhằm cung cấp cho Ban Giám Đốc và các phòng ban liên quan hai chỉ số cốt lõi về vận hành hệ thống y tế:

* **Sức khỏe thương hiệu và lòng trung thành của khách hàng (Retention):** Đánh giá tỷ lệ khách hàng mới, khách hàng quay lại lần thứ hai và tệp khách hàng VIP gắn bó lâu dài. Từ đó đo lường hiệu quả của các chiến dịch Marketing và chất lượng dịch vụ tổng thể.  
* **Năng lực quản trị lâm sàng và tuân thủ điều trị (Follow-up):** Theo dõi sát sao tỷ lệ bệnh nhân quay lại tái khám sau phẫu thuật, phân loại mức độ tuân thủ hẹn khám để kịp thời can thiệp, chăm sóc nhằm đảm bảo an toàn điều trị cao nhất cho người bệnh.

## **2\. HƯỚNG DẪN ĐỌC VÀ HIỂU CÁC SỐ LIỆU TRÊN DASHBOARD**

Để đảm bảo toàn bộ nhân viên các phòng ban có chung một góc nhìn, dưới đây là định nghĩa phi kỹ thuật của từng chỉ số hiển thị trên màn hình:

| Chỉ số trên giao diện | Ý nghĩa thực tế dưới góc nhìn nghiệp vụ | Đơn vị tính   |
| :---- | :---- | :---- |
| **Tổng Lượt Khám (Lũy kế)** | Là tổng số lần đăng ký khám/chữa bệnh được thực hiện tại hệ thống. Nếu một khách hàng đến viện 3 lần trong kỳ lọc, hệ thống sẽ tính là 3 lượt khám. Số liệu này phản ánh quy mô tải vận hành tổng thể của bệnh viện. | Lượt đăng ký |
| **Khách Mới (Đến 1 lần)** | Số lượng khách hàng thực hiện lượt khám đầu tiên trong trong hệ thống trong khoảng thời gian báo cáo.  | Bệnh nhân (Unique) |
| **Khách Quay Lại (Đến 2 lần)** | Số lượng khách hàng quay lại viện và phát sinh lượt khám thứ 2 trong hệ thống (lịch sử tính từ trước đến nay) nằm trong tháng báo cáo. | Bệnh nhân (Unique) |
| **Khách VIP (Đến ≥ 3 lần)** | Số lượng khách hàng trung thành, đã có tổng cộng từ 3 ngày đến viện khám trở lên trong toàn bộ lịch sử và có xuất hiện khám trong kỳ báo cáo.  | Bệnh nhân (Unique) |
| **Quản Trị Tái Khám** | Tỷ lệ phần trăm các ca phẫu thuật/thủ thuật được người bệnh tuân thủ quay lại tái khám kiểm tra trong vòng 1 tháng sau mổ. Chỉ số này phản ánh chất lượng tư vấn của bác sĩ và hiệu quả nhắc hẹn của điều dưỡng/CSKH. | Tỷ lệ phần trăm (%) |
| **Khách Chưa Quay Lại** | Danh sách hoặc số lượng bệnh nhân đã thực hiện phẫu thuật trong kỳ báo cáo nhưng quá hạn vẫn chưa thấy quay lại hệ thống để kiểm tra vết mổ.  | Ca mổ / Bệnh nhân |
| **Hộ Gia Đình Vàng (Lũy kế)** | Chỉ số gom nhóm các khách hàng có mối quan hệ thân nhân (dùng chung một số điện thoại đăng ký). Chỉ số này giúp đánh giá khả năng khai thác dịch vụ theo mô hình gia đình. | Hộ gia đình |

## **3\. DANH MỤC CÁC QUY TẮC TÍNH SỐ LIỆU CẦN THỐNG NHẤT**

#### **1\. Bài toán: Phạm vi định nghĩa "Khách Mới" và "Khách Quay Lại"**

* **Cách tính hiện tại của hệ thống (Tính trên Toàn bộ lịch sử hệ thống):** Hệ thống đang quét toàn bộ lịch sử từ ngày bệnh viện hoạt động để xác định người này đến lần 1 hay lần 2\.  
  * *Ví dụ thực tế:* Bệnh nhân B từng khám Mắt tại viện vào năm 2023\. Đến tháng 5/2026, BN B quay lại khám. Nếu Lãnh đạo lọc xem báo cáo trong tháng 5/2026, hệ thống vẫn nhớ lịch sử năm 2024 và xếp BN B vào tệp *Khách Quay Lại*.  
* **Ý kiến cần thống nhất:** Phòng  Marketing chốt quy tắc tính khách quay lại: Khi khách hàng đến khám nếu sử dụng 1 dịch vụ bất kỳ khác với các dịch vụ đã sử dụng tại LẦN KHÁM LIỀN TRƯỚC thì được tính là khách quay lại.

#### **2\. Bài toán: Cách đếm khách hàng khi Lãnh đạo xem dữ liệu nhiều tháng (Lũy kế)**

* **Cách tính hiện tại của hệ thống (Rủi ro trùng lặp):** Hệ thống đang chấm điểm "Hạng khách hàng" theo từng tháng độc lập rồi cộng dồn cơ học.  
  * *Ví dụ thực tế:* Bệnh nhân A mổ Phaco tháng 1 (Hệ thống đếm là 1 *Khách Mới*). Tháng 6 BN A quay lại cắt kính (Hệ thống đếm là 1 *Khách Quay lại*). Lọc báo cáo "Từ Tháng 1 đến Tháng 6", hệ thống cộng dồn hiển thị bệnh nhân A thành **02 người** ở 2 tệp trên biểu đồ (Khách Mới và Khách Quay lại).  
* **Ý kiến cần thống nhất:** Phòng Marketing đồng ý với cách tính trên hay chuyển sang phương án "Chỉ xét trạng thái cuối cùng". Lọc từ Tháng 1 đến Tháng 6, hệ thống chỉ chốt thứ hạng của BN A vào thời điểm cuối tháng 6\. BN A sẽ chỉ được đếm **01 lần** ở tệp *Khách Quay lại*. Hay Phòng Marketing đề xuất phương án tính mới ?

#### **3\. Bài toán: Phân nhóm thời gian tái khám tuân thủ sau mổ**

* **Cách tính hiện tại của hệ thống :** Trên giao diện đang gom tất cả các ca tái khám thành một nhóm chung là "Đã khám (\<= 1 tháng)".  
  * *Ví dụ :* Khi bệnh nhân sử dụng dịch vụ phẫu thuật tuân thủ đúng phác đồ là khám lại sau 1 ngày. Một bệnh nhân khác mổ xong sau đó 29 ngày mới quay lại. Hệ thống đang đánh giá chung cả hai là "Tuân thủ đúng hẹn".  
* **Ý kiến cần thống nhất :** Phòng Marketing đánh giá có cần chia cụ thể các mốc ngày không hay giữ nguyên như hiện tại ?

#### **4\. Bài toán: Xử lý lượt kiểm tra cận lâm sàng ngay trong ngày phẫu thuật (Khoảng cách \= 0 ngày)**

* **Cách tính hiện tại của hệ thống :** Bộ lọc đang tự động loại bỏ các lượt khám cùng ngày mổ.  
  * *Ví dụ :* Phẫu thuật buổi sáng. Buổi chiều bác sĩ chỉ định siêu âm mắt và đo nhãn áp. Hệ thống ghi nhận có hồ sơ buổi chiều, nhưng **không đếm** là "Đã tái khám thành công" vì diễn ra cùng ngày mổ.  
* **Ý kiến cần thống nhất:** Tiếp tục giữ nguyên logic loại trừ này. "Tuân thủ tái khám" phải là hành vi chủ động quay lại viện vào các ngày tiếp theo (Khoảng cách \>= 1 ngày) hay có trường hợp phải tái khám ngay trong ngày ?

#### **5\. Bài toán: Phân biệt "Khám kiểm tra sau phẫu thuật" với "Khám bệnh mới hoàn toàn"**

* **Cách tính hiện tại của hệ thống :** Hệ thống thấy bệnh nhân có quay lại viện sau ngày phẫu thuật là tự động đếm "Đã tái khám".  
* **Ý kiến cần thống nhất :** Giữ nguyên cách tính như hiện tại hay chỉ những lần đến khám không phát sinh phí (sau khi đã thực hiện phẫu thuật) thì coi là “Đã tái khám”, còn phát sinh phí dịch vụ bất kỳ thì coi là tính là “Khám bệnh mới hoàn toàn” ?  
* Sau khi trao đổi với Phòng kế toán:  
  * Kế toán **không quản lý** tiêu chí "Khách quay lại" theo góc độ giữ chân khách hàng (lòng trung thành).  
  * Kế toán chỉ theo dõi "Khách tái khám" (khám lại đúng bệnh cũ/sau phẫu thuật) dựa trên bộ lọc là **9 mã dịch vụ cụ thể** (gồm 3 dịch vụ thu phí và 6 dịch vụ miễn phí)  
    * GDV2024015	\- Tái khám cho Bệnh nhân viêm trong vòng 3 tuần \- TS.BS Chuyên gia nhãn khoa Liên Bang Nga / 300  
    * GDV2024016	\- Tái khám cho Bệnh nhân viêm trong vòng 3 tuần \- Bác sĩ Chuyên gia nhãn khoa Liên Bang Nga/ 250  
    * GDV2024024	\- Tái khám cho Bệnh nhân viêm trong vòng 3 tuần \- Bác sĩ Việt Nam/ 190  
    * GDV2024018	\- Tái khám sau phẫu thuật Phaco/ dịch kính (trong vòng 6 tháng kể từ ngày PT )   
    * GDV2024019	\- Tái khám sau phẫu thuật Lasik, Phakic  (trong vòng 1 năm kể từ ngày PT)  
    * GDV2024020	\- Tái khám sau laser (trong vòng 14 ngày)  
    * GDV2024021	\- Tái khám sau khóa tập điều trị phục hồi chức năng 0đ  
    * GDV20250315145332202	\- Tái khám sau đặt kính Ortho-K (trong vòng 6 tháng) / 0đ  
    * GDV20250705110832994	\- Tái khám cho BN PT lấy da mi sa/ rút ngắn cơ nâng mi/ lấy bọng mỡ dưới da mi (trong vòng 3 tháng)/ 0
