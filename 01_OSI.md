# Tổng quan mô hình OSI
<img src="https://unifi.vn/wp-content/uploads/2024/03/Mo-hinh-OSI-7-.jpg">
Mô hình OSI (Open Systems Interconnection) là một khung lý thuyết được tổ chức ISO (International Organization for Standardization) phát triển vào cuối những năm 1970 để tiêu chuẩn hóa các chức năng của hệ thống mạng máy tính. Mô hình này chia quá trình giao tiếp mạng thành bảy tầng, mỗi tầng có một chức năng riêng biệt và giao tiếp với các tầng trên hoặc dưới nó.

## Lớp 1 -  Physical 
- Định nghĩa các đặc điểm vật lý của môi trường truyền thông, bao gồm các chuẩn về cáp mạng, tín hiệu điện, tần số, và tốc độ truyền dữ liệu. Xử lý việc truyền tải các bit nhị phân qua môi trường vật lý, như cáp đồng, cáp quang, hoặc sóng vô tuyến. Điều khiển việc kích hoạt, duy trì, và ngắt kết nối vật lý giữa các thiết bị trong mạng
- Giao thức và công nghệ phổ biến:
    - RS-232: Tiêu chuẩn truyền thông nối tiếp giữa các thiết bị như modem và máy tính.
    - Ethernet (cấp vật lý): Định nghĩa các tiêu chuẩn vật lý cho các loại cáp và kết nối dùng trong mạng LAN.
    - Fiber optics: Công nghệ truyền dữ liệu qua các sợi quang, sử dụng ánh sáng để truyền tín hiệu.
- Ví dụ: Khi bạn kết nối một máy tính với router bằng cáp Ethernet, tầng vật lý xử lý việc truyền tải các tín hiệu điện qua cáp để kết nối máy tính với mạng.

## Lớp 2 - Data Link 
- Đảm bảo việc truyền dữ liệu đáng tin cậy giữa hai thiết bị trong cùng một mạng vật lý hoặc mạng cục bộ. Đóng gói dữ liệu thành các khung (frames) để truyền đi và kiểm tra lỗi cơ bản (thường là sử dụng phương pháp CRC - Cyclic Redundancy Check). Điều khiển truy cập tới phương tiện truyền thông, tránh xung đột dữ liệu trên mạng.
- Giao thức phổ biến:
    - Ethernet: Chuẩn phổ biến nhất cho mạng cục bộ (LAN), xác định cách các thiết bị giao tiếp trong cùng một mạng.
    - PPP (Point-to-Point Protocol): Giao thức để thiết lập kết nối trực tiếp giữa hai nút mạng qua một đường truyền điểm tới điểm.
    - MAC (Media Access Control): Quản lý địa chỉ vật lý (MAC address) của các thiết bị trong mạng và điều khiển truy cập tới phương tiện truyền thông.
- Ví dụ: Khi bạn kết nối máy tính với mạng Wi-Fi, tầng liên kết dữ liệu sử dụng địa chỉ MAC để xác định các thiết bị và quản lý giao tiếp giữa chúng.

## Lớp 3 - Network 
- Định tuyến các gói dữ liệu từ nguồn đến đích qua một hoặc nhiều mạng trung gian, đảm bảo gói dữ liệu đến đúng địa chỉ. Quản lý địa chỉ IP, phân mảnh các gói dữ liệu và lắp ghép lại khi đến đích. Điều khiển lưu lượng và tránh tắc nghẽn trong mạng.
- Giao thức phổ biến:
    - IP (Internet Protocol): Địa chỉ và định tuyến các gói tin trên mạng
    - ICMP (Internet Control Message Protocol): Dùng để truyền tải các thông báo lỗi và điều khiển trong mạng.
- Ví dụ: Khi bạn gửi một email, tầng mạng sử dụng giao thức IP để định tuyến gói dữ liệu từ máy tính của bạn qua nhiều mạng khác nhau đến máy chủ nhận.


## Lớp 4 - Transport Layer
- Cung cấp dịch vụ truyền tải dữ liệu đáng tin cậy hoặc không đáng tin cậy giữa hai hệ thống cuối. Quản lý việc phân đoạn dữ liệu và tái lắp ghép dữ liệu tại điểm đích, đảm bảo dữ liệu đến đúng thứ tự. Kiểm soát lưu lượng và đảm bảo rằng dữ liệu không bị mất hoặc bị trùng lặp.
- Giao thức phổ biến:
    - TCP (Transmission Control Protocol): Cung cấp truyền tải dữ liệu đáng tin cậy, kiểm soát lỗi và kiểm soát luồng.
    - UDP (User Datagram Protocol): Cung cấp truyền tải dữ liệu không đáng tin cậy, không có kiểm soát lỗi, thích hợp cho các ứng dụng yêu cầu tốc độ cao, như streaming video.
- Ví dụ: Khi bạn tải xuống một file từ internet, giao thức TCP sẽ đảm bảo rằng toàn bộ file được tải xuống một cách toàn vẹn và theo đúng thứ tự.

## Lớp 5 - Session 
- Quản lý các phiên giao tiếp giữa hai hệ thống, đảm bảo rằng các phiên này được thiết lập, duy trì, và kết thúc đúng cách. Đồng bộ hóa luồng dữ liệu, quản lý giao tiếp và đảm bảo dữ liệu không bị mất mát. Điều khiển việc giao tiếp liên tục giữa hai hệ thống trong suốt thời gian phiên, có thể bao gồm cả việc phục hồi sau lỗi.
- Giao thức phổ biến:
    - RPC (Remote Procedure Call): Giao thức cho phép một chương trình thực hiện một hàm hoặc thủ tục trên một máy tính từ xa.
    - NetBIOS: Giao thức hỗ trợ phiên, quản lý việc kết nối và duy trì các phiên trong mạng.
- Ví dụ: Khi bạn thực hiện một giao dịch ngân hàng trực tuyến, tầng phiên đảm bảo rằng phiên giao dịch của bạn được duy trì liên tục và an toàn.

## Tầng 6 - Presentation 
- Đảm bảo rằng dữ liệu được truyền từ tầng ứng dụng của một hệ thống có thể được hiểu bởi tầng ứng dụng của hệ thống khác, dù chúng có định dạng dữ liệu khác nhau. Xử lý việc mã hóa/giải mã dữ liệu để bảo mật. Thực hiện nén và giải nén dữ liệu để giảm kích thước truyền tải và tối ưu hóa băng thông.
- Giao thức phổ biến:
    - SSL/TLS: Giao thức bảo mật tầng transport được dùng để mã hóa dữ liệu trong các kết nối web an toàn.
    - MIME (Multipurpose Internet Mail Extensions): Định dạng dữ liệu được dùng trong email để hỗ trợ nội dung đa phương tiện.
- Ví dụ: Khi bạn truy cập một trang web HTTPS, dữ liệu được mã hóa bằng TLS ở tầng trình bày để bảo vệ thông tin truyền tải giữa trình duyệt và máy chủ.

## Tầng 7 -  Application 
- Cung cấp giao diện trực tiếp giữa ứng dụng người dùng và các dịch vụ mạng. Xử lý các giao thức cấp cao, giao tiếp trực tiếp với người dùng cuối thông qua các ứng dụng như trình duyệt web, email, hay truyền tải file. Quản lý giao tiếp giữa các ứng dụng khác nhau và mạng, xử lý yêu cầu của người dùng (ví dụ: truy xuất dữ liệu từ một máy chủ web).
- Giao thức phổ biến:
    - HTTP/HTTPS: Giao thức truyền tải siêu văn bản, được sử dụng bởi các trình duyệt web để tải trang web.
    - FTP (File Transfer Protocol): Giao thức truyền tải file qua mạng.
    - SMTP (Simple Mail Transfer Protocol): Giao thức truyền tải email.
    - DNS (Domain Name System): Giao thức chuyển đổi tên miền thành địa chỉ IP.
- Ví dụ: Khi bạn nhập một URL vào trình duyệt, trình duyệt sẽ sử dụng giao thức HTTP để yêu cầu và nhận trang web từ máy chủ.

### Ví dụ luồng hoạt động của app outlook
- Step 1: Mở outlook ra, bằng việc kích đúp icon, giao diện sẽ mở -> việc mở ra giao diện ở tầng application
- Step 2: Chọn new email, điền mail nhận, gõ text, hình ảnh, file đính kèm -> tầng presentation
- Step 3: Sau khi bấm send mail, bản tin mail sẽ được gắn ip , port của mail server, ip source người gửi =>thực hiện tại tầng session
- Step 4: Cắt nhỏ bản tin thành các phần nhỏ (segment), mỗi phần được dán port src , port dst ở trên vào => thực hiện ở tầng transport. ␋
- Step 5: Dán tiếp IP source, đích vào (gọi là packet), tìm đường đi đến IP đích, dựa vào bảng routing table => thực hiện tại tầng network, kết quả tìm ra interface nào cần đẩy ra,
- Step 6: Thực hiện việc dán địa chỉ source MAC (của cổng mạng) và dest MAC (là MAC của thiết bị gần nhất) , gọi là frame => thực hiện tại Tầng data link
- Step 7: Biến các frame thành tín hiệu điện và gửi lên cable mạng truyền đi đến đích => Physical



