---
layout: post
title: "DNS over HTTPS là gì? Hiểu Đúng Về DoH Và Cách Bật Trên Mọi Thiết Bị"
date: 2026-04-21
---

Mỗi lần bạn gõ một địa chỉ web, thiết bị của bạn thực hiện một tra cứu DNS âm thầm và thường là hoàn toàn không được mã hóa. DNS over HTTPS (DoH) ra đời để thay đổi điều đó, bảo vệ quyền riêng tư của bạn ngay từ bước đầu tiên kết nối internet.


## **DNS over HTTPS Là Gì và Tại Sao Nó Ra Đời**

Để hiểu **dns over HTTPS là gì**, trước tiên cần nắm DNS hoạt động như thế nào. DNS (Domain Name System) là hệ thống "danh bạ" của internet, chuyển đổi tên miền dễ nhớ như _google.com_ thành địa chỉ IP mà máy chủ hiểu được.

Vấn đề nằm ở chỗ: trong nhiều thập kỷ, các truy vấn DNS truyền đi dưới dạng văn bản thuần túy, không có bất kỳ lớp mã hóa nào. Điều này có nghĩa là nhà cung cấp dịch vụ internet (ISP), quản trị viên mạng, hoặc bất kỳ ai ngồi giữa đường truyền đều có thể nhìn thấy bạn đang truy cập những tên miền nào, dù trang web đó có dùng HTTPS hay không.

DNS over HTTPS (DoH) được chuẩn hóa qua RFC 8484 năm 2018, giải quyết đúng vấn đề này. DoH gửi các truy vấn DNS thông qua giao thức HTTPS, tức là toàn bộ yêu cầu phân giải tên miền được mã hóa, trộn lẫn với lưu lượng web thông thường trên cổng 443, không thể phân biệt từ bên ngoài.


## **Cơ Chế Hoạt Động Của DNS over HTTPS**

Quy trình DoH diễn ra hoàn toàn trong nền, người dùng không cần làm gì thủ công sau khi đã bật. Các bước cụ thể như sau:

1. **Khởi tạo truy vấn:** Khi bạn nhập địa chỉ web, trình duyệt tạo một truy vấn DNS để tìm địa chỉ IP tương ứng.

2. **Đóng gói và mã hóa:** Truy vấn được đóng gói thành một yêu cầu HTTPS và mã hóa bằng TLS, giống như bất kỳ giao tiếp web bảo mật nào.

3. **Gửi đến máy chủ DoH:** Yêu cầu này được chuyển đến một máy chủ DNS có hỗ trợ DoH, ví dụ như của Cloudflare (1.1.1.1), Google (8.8.8.8) hoặc Quad9 (9.9.9.9).

4. **Phân giải và phản hồi:** Máy chủ DoH giải mã yêu cầu, thực hiện phân giải tên miền, rồi trả kết quả về dưới dạng mã hóa qua HTTPS.

5. **Kết nối:** Thiết bị nhận phản hồi, giải mã địa chỉ IP và tiến hành kết nối đến trang web mong muốn.

Điểm mấu chốt là toàn bộ quá trình này đều được bảo vệ, không ai ngồi giữa đường truyền có thể đọc nội dung truy vấn DNS của bạn.


## **Lợi Ích Thực Sự Khi Dùng DNS over HTTPS**

DoH không chỉ là một tính năng kỹ thuật trừu tượng, nó mang lại những lợi ích rõ ràng và thiết thực trong cuộc sống số hằng ngày:

- **Bảo vệ lịch sử duyệt web:** ISP và các bên thứ ba không còn nhìn thấy bạn đang truy cập domain nào, ngay cả khi bạn dùng Wi-Fi công cộng.

- **Chống tấn công DNS Spoofing và MITM:** Vì truy vấn được mã hóa và gửi đến máy chủ đáng tin cậy, kẻ tấn công khó có thể chèn phản hồi giả mạo để điều hướng bạn đến website độc hại.

- **Vượt qua kiểm duyệt DNS:** Một số nội dung bị chặn ở cấp độ DNS có thể truy cập được khi dùng DoH, vì ISP không còn thấy bạn đang hỏi về tên miền nào.

- **Tính toàn vẹn của dữ liệu DNS:** Phản hồi DNS nhận được khó bị thay đổi trong quá trình truyền tải so với DNS thông thường.

- **Tích hợp sẵn, không cần phần mềm phụ:** Các trình duyệt hiện đại như Chrome, Firefox, Edge đều hỗ trợ DoH ngay trong cài đặt, không cần cài thêm bất cứ thứ gì.


## **Hạn Chế Cần Biết Trước Khi Bật DoH**

DoH tốt, nhưng không hoàn hảo. Dưới đây là những điểm cần cân nhắc:

- **Tập trung hóa dữ liệu DNS:** Khi dùng DoH qua Cloudflare hoặc Google, bạn đang chuyển sự tin tưởng từ ISP sang các công ty lớn này. Dữ liệu truy vấn DNS của bạn vẫn đi qua tay họ.

- **Phá vỡ bộ lọc mạng nội bộ:** Trong môi trường công ty hoặc trường học, DoH có thể bỏ qua DNS nội bộ, làm vô hiệu hóa bộ lọc nội dung hoặc kiểm soát truy cập.

- **Khó chẩn đoán sự cố mạng:** Quản trị viên IT khó kiểm tra và phân tích lưu lượng DNS khi nó được mã hóa hoàn toàn.

- **Có thể tăng nhẹ độ trễ:** Quá trình mã hóa và thiết lập kết nối TLS thêm một chút overhead, dù trong thực tế sự khác biệt thường không đáng kể.


## **Cách Bật DNS over HTTPS Trên Trình Duyệt**

### **Google Chrome**

- Nhấn vào biểu tượng ba chấm, chọn **Settings**.

- Vào **Privacy and Security → Security**, kéo xuống phần **Advanced**.

- Bật **Use secure DNS** và chọn nhà cung cấp như Cloudflare hoặc Google, hoặc nhập DNS tùy chỉnh.


### **Mozilla Firefox**

- Nhập **about:preferences** vào thanh địa chỉ.

- Vào **Privacy & Security**, kéo xuống mục **DNS over HTTPS**.

- Chọn **Increased Protection** hoặc **Max Protection** tùy mức độ bảo mật muốn.

- Chọn nhà cung cấp DNS (Cloudflare, NextDNS) hoặc nhập tùy chỉnh.


### **Microsoft Edge**

- Nhập **edge://settings/privacy** vào thanh địa chỉ.

- Cuộn xuống phần **Security**, bật **Use secure DNS**.

- Chọn nhà cung cấp DNS hỗ trợ DoH phù hợp.


## **Cách Bật DNS over HTTPS Trên Hệ Điều Hành**

### **Windows 11**

- Vào **Settings → Network & Internet**, chọn Wi-Fi hoặc Ethernet đang dùng.

- Nhấn vào mạng đang kết nối, chọn **Hardware properties**.

- Tại **DNS server assignment**, nhấn **Edit**, chọn **Manual** và bật IPv4.

- Nhập DNS hỗ trợ DoH: Cloudflare (**1.1.1.1** và **1.0.0.1**), Google (**8.8.8.8** và **8.8.4.4**), hoặc Quad9 (**9.9.9.9**).

- Ở mục **DNS encryption**, chọn **Encrypted only (DNS over HTTPS)** rồi nhấn Save.


### **macOS**

- Mở **System Settings → Network**, chọn kết nối Wi-Fi hoặc Ethernet.

- Nhấn **Details…**, chuyển sang tab **DNS Servers**.

- Nhấn dấu **+** và thêm địa chỉ DNS hỗ trợ DoH như **1.1.1.1** và **1.0.0.1**.

- Nhấn OK rồi Apply để lưu thay đổi.

**Kiểm tra DoH đã hoạt động chưa:** Truy cập **https\://1.1.1.1/help**, mục "Using DNS over HTTPS" sẽ hiển thị trạng thái Yes hoặc No rõ ràng.


## **DoH Khác Gì Với DNSSEC, DNS over TLS và DNSCrypt**

Nhiều người dùng nhầm lẫn giữa các giao thức DNS bảo mật. Đây là cách phân biệt nhanh:

- **DNS over HTTPS (DoH):** Mã hóa truy vấn qua cổng HTTPS (443), khó bị chặn, phù hợp cho người dùng cá nhân và trình duyệt.

- **DNS over TLS (DoT):** Cũng mã hóa truy vấn nhưng dùng cổng riêng 853, dễ nhận biết và dễ bị tường lửa chặn hơn, thường dùng trong môi trường doanh nghiệp.

- **DNSCrypt:** Xác thực và mã hóa truy vấn bằng chữ ký số, bảo mật cao nhưng chưa được hỗ trợ rộng rãi trên các trình duyệt và hệ điều hành phổ biến.

- **DNSSEC:** Không mã hóa nội dung truy vấn, chỉ xác minh rằng phản hồi DNS đến từ nguồn hợp lệ, không bảo vệ quyền riêng tư người dùng.

DoH và DoT đều mã hóa, nhưng DoH phù hợp cho người dùng thông thường hơn vì hoạt động ngầm trong trình duyệt mà không cần cấu hình hệ thống phức tạp.


## **Kết Luận**

**DNS over HTTPS là gì**, giờ bạn đã có câu trả lời đầy đủ. Đây là một bước tiến quan trọng trong hành trình bảo vệ quyền riêng tư trực tuyến, che giấu lịch sử duyệt web của bạn ngay từ tầng DNS, nơi mà HTTPS thông thường không thể can thiệp.

Việc bật DoH trên Chrome, Firefox hay Windows 11 chỉ mất vài phút nhưng mang lại lớp bảo vệ thực chất. Nếu bạn thường xuyên dùng Wi-Fi công cộng, làm việc với dữ liệu nhạy cảm, hoặc đơn giản là coi trọng quyền riêng tư số, bật DoH là một trong những việc đơn giản và hiệu quả nhất bạn có thể làm ngay hôm nay.

\>> Xem thêm: <https://topon.tech/vi/dns-over-https-la-gi/>
