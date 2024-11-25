# Định tuyến 
## 1.Khái niệm
- Định tuyến là quá trình chọn và xác định đường đường đi tối ưu cho dữ liệu được truyền qua mạng từ nguồn đến đích.
- Bảng định tuyến (Routing Table): là bảng chứa tất cả những đường đi tốt nhất đến một đích nào đó tính từ router. Khi cần chuyển tiếp một gói tin, router sẽ xem địa chỉ đích của gói tin, sau đó tra bảng định tuyến và chuyển gói tin đi theo đường tốt nhất tìm được trong bảng. Trong bảng định tuyến có thể bao gồm một tuyến mặc định, được biểu diễn bằng địa chỉ 0.0.0.0 0.0.0.0.
- Bảng định tuyến của mỗi giao thức định tuyến là khác nhau, nhưng có thể bao gồm những thông tin sau:
  - Địa chỉ mạng đích (Destination Network) là địa chỉ của mạng mà các gói tin hướng đến.
  - Mặt nạ mạng (Subnet Mask hoặc Prefix Length) xác định phạm vi địa chỉ thuộc về mạng đích.
  - Cổng tiếp theo (Next Hop) Là địa chỉ IP của router tiếp theo mà gói tin cần được chuyển tới.
  - Giao diện mạng (Outgoing Interface) Giao diện vật lý hoặc logic mà gói tin sẽ được gửi qua.
  - Trọng số (Metric) Giá trị để đánh giá mức độ "ưu tiên" của tuyến đường. Metric thấp hơn thì tuyến đường được ưu tiên hơn.
  - Loại giao thức (Protocol Type) Chỉ ra giao thức định tuyến nào đã thêm tuyến đường này vào bảng.
  - Thời gian tồn tại (Timer hoặc Age) Thời gian đã trôi qua kể từ khi tuyến đường được thêm vào hoặc xác nhận lần cuối.Giao thức động như RIP hoặc OSPF thường sử dụng thông tin này để loại bỏ tuyến đường không hợp lệ.
## 2.Phân loại
- Định tuyến tĩnh là định tuyến mà trong đó router sử dụng các tuyến đường đi tĩnh để vận chuyển dữ liệu. Các tuyến đường được cấu hình thủ công bởi quản trị viên mạng.
- Định tuyến động là định tuyến mà các router tự trao đổi thông tin về các mạng. Tự chạy một phương thức tính toán nào đó để xác định xem để đi đến các mạng này thì phải sử dụng đường đi nào tối ưu. Các router cần phải chạy giao thức định tuyến để có thể tương tác trao đổi thông tin và tính toán Routing. Việc xây dựng và duy trì trạng thái của bảng định tuyến được thực hiện tự động bởi router.

  Định tuyến động có thể được phân loại dựa trên cách hoạt động của các giao thức định tuyến. Dưới đây là các loại chính:
  - Distance-Vector: các router chia sẻ thông tin định tuyến với router lân cận bằng cách gửi toàn bộ hoặc một phần bảng định tuyến. Khoảng cách đến mạng đích được đo bằng số bước nhảy (hop count) hoặc các tiêu chí tương tự. Router không có thông tin toàn bộ về mạng, chỉ biết thông tin từ các router láng giềng. Ưu điểm: Dễ cấu hình và triển khai, phù hợp cho các mạng nhỏ. Nhược điểm: Hội tụ chậm khi mạng thay đổi, tiềm ẩn vấn đề như vòng lặp định tuyến (routing loops).
  - Link-State: Mỗi router có thông tin đầy đủ về cấu trúc toàn bộ mạng thông qua các thông báo trạng thái liên kết (LSA - Link State Advertisement). Sử dụng thuật toán Dijkstra để tìm đường đi ngắn nhất (SPF - Shortest Path First). Hội tụ nhanh hơn, đáng tin cậy và hiệu quả trong các mạng lớn, phức tạp. Nhược điểm: Yêu cầu nhiều tài nguyên hơn (CPU, RAM), cấu hình phức tạp hơn.
  - Hybrid: Kết hợp ưu điểm của Distance-Vector và Link-State. Sử dụng đặc điểm tự động học và cập nhật tuyến đường như Distance-Vector nhưng cũng có khả năng hội tụ nhanh và hiệu quả như Link-State. Ưu điểm: Linh hoạt, hiệu quả, phù hợp với các mạng lớn, đa dạng. Nhược điểm: Yêu cầu tài nguyên cao hơn Distance-Vector, cần hiểu biết sâu về cấu hình.
- Hai tham số dùng trong định tuyến: Metric và Ad
  - Metric là một giá trị được các giao thức định tuyến sử dụng để đánh giá và so sánh chất lượng của các tuyến đường. Tuyến đường có metric thấp hơn sẽ được ưu tiên. Tùy thuộc vào giao thức định tuyến, metric có thể dựa trên các yếu tố sau: Số bước nhảy (Hop Count), băng thông (Bandwidth), độ trễ (Delay), độ tin cậy (Reliability), chi phí (Cost).
  - AD (Administrative Distance) là giá trị ưu tiên của các giao thức định tuyến trên một router, được sử dụng khi có nhiều giao thức cùng cung cấp tuyến đường đến một đích. Tuyến đường từ giao thức có AD thấp hơn sẽ được ưu tiên.
## 3.Các giao thức định tuyến phổ biến
### 3.1 RIP
- là một giao thức định tuyến theo kiểu "distance-vector". Hop count được sử dụng làm metric cho việc chọn đường. Nếu có nhiều đường đến cùng một đích thì RIP sẽ chọn đường nào có số hop count (số router đi qua) ít nhất. RIP hỗ trợ tính năng chia tải (load balancing) mặc định là 4 đường, tối đa là 16 đường. Nếu hop count lớn hơn 15 thì các gói tin sẽ bị loại bỏ. Mặc định thời gian cập nhật định tuyến là 30 giây. Giá trị AD mặc định của RIP là 120.
- Các phiên bản của RIP:
  - RIP v1 (RFC 1058): Hỗ trợ định tuyến không phân lớp (Classful Routing). Không gửi thông tin subnet mask, không hỗ trợ VLSM (Variable Length Subnet Mask). Không hỗ trợ xác thực (Authentication).
  - RIP v2 (RFC 2453): Hỗ trợ định tuyến phân lớp (Classless Routing). Gửi thông tin subnet mask, hỗ trợ VLSM. Hỗ trợ xác thực bằng mật khẩu (Authentication). Tương thích ngược với RIP v1.
  - RIPng (RIP Next Generation) (RFC 2080): Phiên bản dành cho IPv6. Không hỗ trợ xác thực nội tại (sử dụng IPsec để bảo mật). Gửi thông tin định tuyến qua nhóm multicast FF02::9.
### 3.2 OSPF
- OSPF (Open Shortest Path First) là một giao thức định tuyến dạng link-state, sử dụng thuật toán Dijkstra để xây dựng bảng định tuyến.OSPF mang những đặc điểm của giao thức link-state. Nó có ưu điểm là hội tụ nhanh, hỗ trợ được mạng có kích thước lớn và không xảy ra "routing loop". OSPF sử dụng địa chỉ multicast 224.0.0.5 và 224.0.0.6 (DR và BDR router) để gửi các thông điệp hello và update trong quá trình cập nhật định tuyến. Bên cạnh đó, OSPF còn được thiết kế theo dạng phân cấp, sử dụng các area để giảm yêu cầu về CPU, bộ nhớ của router. OSPF hỗ trợ chứng thực dạng Plain-Text và MD5.
- Metric của OSPF OSPF sử dụng metric là cost. Cost của toàn tuyến đường được tính theo cách cộng dồn cost dọc theo tuyến đường đi của packet. Cách tính cost được IETF đưa ra trong RFC 2328. Cost được tính dựa trên bảng thông sao cho tốc độ kết nối của đường kết nối càng cao thì cost càng thấp dựa trên công thức 10^8 / bandwidth với giá trị bandwidth được cấu hình trên mỗi cổng của router và đơn vị tính là bps. Tuy nhiên, chúng ta có thể thay đổi giá trị cost. Nếu router có nhiều đường đến đích mà chi phí bằng nhau thì router sẽ cân bằng tải trên các đường đó, mặc định trên 4 đường, tối đa là 16 đường. Những tham số bắt buộc phải giống nhau trong các router chạy OSPF trong một hệ thống mạng, đó là Hello/dead interval, Area-ID, authentication password (nếu có), stub area flag.
- Hoạt động của giao thức OSPF
  - Hoạt động của giao thức OSPF
  - Chọn Router-ID(giá trị duy nhất dùng để định danh cho các router chạy OSPF trong cùng 1 phân vùng, có định dạng là 1 địa chỉ IP). Mặc định tiến trình OSPF trên router sẽ bầu chọn địa chỉ IP cao nhất trên các cổng đang active, ưu tiên cổng loopack.    

    Có một cách khác để thiết lập lại giá trị router – id là sử dụng câu lệnh “router-id” để thiết lập bằng tay giá trị này trên router:  

    `Router (config) # router ospf 1`  
    `Router (config-router) # router-id A.B.C.D`

    Bên cạnh đó, nếu tiến trình OSPF đã chạy và router – id đã được thiết lập trước đó, ta phải khởi động lại tiến trình OSPF thì mới áp dụng được giá trị router – id mới được chỉ ra trong câu lệnh “router – id”. Câu lệnh khởi động lại tiến trình OSPF:

    `Router (config) # clear ip ospf proccess`  
  `Reset ALL OSPF proccess? [no]: yes`  
   - Thiết lập quan hệ láng giềng: router chạy OSPF sẽ gửi gói tin Hello tới tất cả các cổng chạy ospf, mặc định 10s/lần. Gói tin này được gửi đến địa chỉ multicast dành riêng cho ospf là 224.0.0.5, đến tất cả các router chạy ospf khác trên cùng 1 phân đoạn mạng. Mục đích của gói tin Hello là giúp các router tìm kiếm, thiết lập và duy trì quan hệ láng giềng.  
   - Trao đổi LSDB(Link State Database): LSDB là một tấm bản đồ mạng và router sẽ căn cứ vào đó để xác định bảng định tuyến. LSDB giống nhau giữa tất cả các router cùng vùng. Các router sẽ không trao đổi với nhau cả bảng LSDB mà sẽ trao đổi với nhau từng đơn vị thông tin gọi là LSA(Link State Advertisement). Các đơn vị thông tin này lại được chứa trong các gói tin cụ thể LSU(Link State Update) mà các router sẽ trực tiếp trao đổi với nhau.
### 3.3 EIGRP
- EIGRP (Enhanced Interior Gateway Routing Protocol) là một giao thức định tuyến hybrid, kết hợp giữa distance-vector và link-state, được Cisco phát triển. EIGRP được thiết kế để cung cấp một cách thức hiệu quả và nhanh chóng trong việc tính toán và duy trì bảng định tuyến trong mạng.
- Công thức tính metric:  
```
metric = ([K1 * bandwidth + (K2 * bandwidth) / (256 - load) + K3 * delay] * [K5 / (reliability + K4)]) * 256  
```
  *Trong đó:*
  - `Bandwitdh` ( băng thông ) đơn vị là `kbps`  
  - `Delay` ( độ trễ ) đơn vị là `10 microsecond`
  - `Load` ( khả năng truyền tải ) , `Reliability` ( độ tin cậy ) là các đại lượng vô hướng không có đơn vị .
  - Giá trị k mặc định là `k1 = k3 = 1` ; `k2 = k4 = k5 = 0`

==> Công thức mặc định:  
     `metric = bandwidth + delay`  
