# VLAN
## 1.Tổng quan
- Miền đụng độ - Collision domain
  - Là phạm vi mà các gói tin có thể xảy ra xung đột (collision) khi hai hoặc nhiều thiết bị cố gắng truyền dữ liệu đồng thời trên cùng một kênh.
  - Xảy ra ở tầng 2 của mô hình OSI (Data Link Layer), chủ yếu trong các mạng sử dụng Hub hoặc Ethernet cũ.
  - Khi đụng độ xảy ra , các gói tin đang được truyền đều bị phá hủy , các máy truyền sẽ ngưng việc truyền dữ liệu và chờ 1 khoảng thời gian ngẫu nhiên theo quy luật của CSMA / CD ( Carrier Sense Multiple Access with Collision Detection )
  - Switch và Bridge giúp chia nhỏ miền đụng độ, mỗi cổng của Switch là một miền đụng độ riêng biệt.
- Miền quảng bá - Broadcast Doamin
  - Là phạm vi mà một gói tin quảng bá (broadcast) được truyền tới tất cả các thiết bị trong mạng.
  - Xảy ra khi một thiết bị gửi gói tin tới địa chỉ MAC Broadcast (FF:FF:FF:FF:FF:FF).
  - Router chia nhỏ miền quảng bá, còn Switch hoạt động trong cùng một miền quảng bá (trừ khi có cấu hình VLAN).
- VLAN (Virtual Local Area Network) là một công nghệ cho phép tạo các mạng LAN ảo độc lập trong cùng một thiết bị mạng vật lý, như Switch. Tưởng tượng bạn có một tòa nhà với nhiều phòng. Mỗi phòng đại diện cho một VLAN. Mặc dù các phòng nằm trong cùng một tòa nhà (mạng vật lý), nhưng người ở các phòng khác nhau không thể truy cập trực tiếp vào máy tính của người ở phòng khác trừ khi có sự cho phép đặc biệt. VLAN có tác dụng:
  - Phân chia mạng logic: Tách các nhóm thiết bị thành các mạng riêng biệt dựa trên chức năng, bộ phận, hoặc bảo mật, mà không cần thay đổi phần cứng.
  - Giảm miền quảng bá: Mỗi VLAN là một miền quảng bá riêng, giảm tải lưu lượng không cần thiết trên mạng.
  - Cải thiện bảo mật: Các VLAN riêng biệt không thể giao tiếp với nhau trừ khi có cấu hình thêm (VD: qua Router hoặc Layer 3 Switch).
  - Quản lý linh hoạt: Dễ dàng thêm, xóa, hoặc di chuyển thiết bị giữa các VLAN mà không cần thay đổi cáp mạng.
- Phân loại:
  - VLAN tĩnh (Static VLAN) là loại VLAN mà người quản trị mạng trực tiếp cấu hình các cổng trên switch để gán vào một VLAN cụ thể. Nghĩa là, mỗi cổng sẽ được gắn cố định vào một VLAN và không thay đổi trừ khi người quản trị có ý định thay đổi.
  - VLAN động (Dynamic VLAN) là loại VLAN mà việc gán một cổng vào VLAN được xác định dựa trên các thông tin động như địa chỉ MAC của thiết bị kết nối. Quá trình này thường được thực hiện tự động bởi switch thông qua một cơ chế quản lý trung tâm.

     | Tính năng | VLAN tĩnh | VLAN động |
     |---|---|---|
     | Cấu hình | Trực tiếp bởi quản trị viên | Tự động dựa trên thông tin thiết bị |
     | Linh hoạt | Thấp | Cao |
     | Quản lý | Dễ | Khó hơn |
     | Ổn định | Cao | Thấp hơn (có thể xảy ra lỗi nếu cấu hình không đúng) |
     | Phù hợp | Mạng có cấu hình tĩnh, ít thay đổi | Mạng có nhiều thiết bị di động |

- Trunk port:
  - Xử lý nhiều VLAN: Cổng Trunk cho phép lưu lượng từ nhiều VLAN đi qua.
  - Gắn thẻ VLAN (Tagged): Sử dụng tiêu chuẩn IEEE 802.1Q để gắn thẻ VLAN vào các gói tin, giúp phân biệt lưu lượng của các VLAN.
  - Dùng cho kết nối giữa các thiết bị mạng: Switch-Switch, Switch-Router, hoặc Switch-Server.
- Access port:
  - Thuộc về một VLAN duy nhất: Cổng Access chỉ xử lý lưu lượng của một VLAN cụ thể.
  - Không gắn thẻ VLAN (Untagged): Các gói tin truyền qua cổng Access không được gắn thẻ VLAN.
  - Dùng cho thiết bị đầu cuối: Máy tính, máy in, điện thoại IP thường kết nối qua cổng Access.
## 2.Giao thức VTP (Vlan Trunking Protocol)
- là một giao thức quản lý VLAN trong các mạng Switch Cisco, cho phép tự động chia sẻ thông tin VLAN giữa các Switch trong cùng một miền quản trị (VTP domain).
- Mục đích của VTP:
  - Quản lý tập trung VLAN: Tạo, chỉnh sửa hoặc xóa VLAN trên một Switch sẽ tự động cập nhật các Switch khác trong cùng miền VTP.
  - Giảm công sức cấu hình: Hạn chế lỗi do cấu hình thủ công trên từng Switch.
  - Đồng bộ hóa VLAN: Đảm bảo mọi Switch trong miền VTP có thông tin VLAN nhất quán.
- Các chế độ của VTP
  - Server Mode: Switch trong chế độ này có thể tạo, chỉnh sửa, hoặc xóa VLAN. Các thay đổi được gửi tới tất cả Switch khác trong miền VTP. Mặc định, Switch Cisco hoạt động ở chế độ Server.
  - Client Mode: Switch trong chế độ này không thể tạo, chỉnh sửa, hoặc xóa VLAN. Chỉ nhận thông tin VLAN từ các Switch ở chế độ Server.
  - Transparent Mode: Switch ở chế độ này không tham gia đồng bộ hóa VLAN qua VTP. Vẫn có thể tạo và chỉnh sửa VLAN, nhưng chỉ áp dụng trên chính Switch đó. Vẫn chuyển tiếp thông điệp VTP tới các Switch khác trong miền.
- Cơ chế hoạt động:
  - VTP gửi thông điệp quảng bá qua “VTP domain” mỗi 5 phút một lần, hoặc khi có sự thay đổi xảy ra trong cấu hình VLAN. Một thông điệp VTP bao gồm “revision-number”, tên VLAN (VLAN name), số hiệu VLAN (VLAN number), và thông tin về các switch có cổng gắn với mỗi VLAN. Bằng sự cấu hình VTP Server và việc quảng bá thông tin VTP, tất cả các switch đều đồng bộ về tên VLAN và số hiệu VLAN của tất cả các VLAN.
  - Một trong những thành phần quan trọng trong các thông tin quảng bá VTP là tham số “revision number”. Mỗi lần VTP Server điều chỉnh thông tin VLAN, nó tăng “revision-number” lên 1, rồi sau đó VTP Server mới gửi thông tin quảng bá VTP đi. Khi một switch nhận một thông điệp VTP với “revision-number” lớn hơn, nó sẽ cập nhật cấu hình VLAN.
## 3.Giao thức STP (Spanning Tree Protocol)
- là một giao thức được sử dụng để ngăn ngừa hiện tượng lặp vô hạn (loop) trong mạng Ethernet khi có các kết nối mạng dư thừa, chẳng hạn như giữa các switch. STP giúp đảm bảo rằng mạng có thể hoạt động ổn định mà không gặp phải vấn đề mạng bị "tắc nghẽn" do các vòng lặp, và đồng thời cung cấp các kết nối dự phòng khi các liên kết chính gặp sự cố.
- STP hoạt động qua các bước sau:
  - Bầu chọn Root Bridge: Mỗi switch trong mạng sẽ gửi đi các thông điệp đặc biệt gọi là BPDU (Bridge Protocol Data Unit) để thông báo cho các switch khác về thông tin của mình, bao gồm Bridge ID (gồm Bridge Priority và địa chỉ MAC). Switch có Bridge ID nhỏ nhất sẽ được chọn làm Root Bridge. Bridge Priority có thể được cấu hình thủ công, nhưng giá trị mặc định của nó là 32768.
  - Xác định Root Port: Mỗi switch sẽ chọn một cổng để kết nối đến Root Bridge, cổng này được gọi là Root Port. Root Port là cổng có chi phí đường đi đến Root Bridge nhỏ nhất.
  - Xác định Designated Port: Trên mỗi phân đoạn mạng (segment), switch có Root Port nhỏ nhất sẽ được chọn làm Designated Port. Designated Port là cổng được sử dụng để chuyển tiếp khung dữ liệu trên phân đoạn đó.
  - Xác định Designated Port: Trên mỗi phân đoạn mạng (segment), switch có Root Port nhỏ nhất sẽ được cChặn các cổng không cần thiết: Các cổng còn lại trên switch sẽ bị chặn (Blocking) để ngăn chặn việc hình thành vòng lặp.ọn làm Designated Port. Designated Port là cổng được sử dụng để chuyển tiếp khung dữ liệu trên phân đoạn đó.
- Quá trình bầu chọn Root Bridge:
  - Bridge ID: Mỗi switch có một Bridge ID duy nhất, bao gồm: Bridge Priority: Giá trị này có thể được cấu hình thủ công để ưu tiên một switch làm Root Bridge. Địa chỉ MAC: Nếu nhiều switch có cùng Bridge Priority, địa chỉ MAC sẽ được sử dụng để so sánh.
  - BPDU: Các switch sẽ gửi đi các BPDU để quảng bá thông tin về Bridge ID của mình.
  - So sánh Bridge ID: Các switch khác sẽ so sánh Bridge ID của mình với các BPDU nhận được và chọn Root Bridge có Bridge ID nhỏ nhất.
## 4.Định tuyến giữa các VLAN
### Định tuyến giữa các VLAN bằng Router (Router-on-a-Stick)
- Router-on-a-Stick là một phương pháp truyền thống để thực hiện định tuyến giữa các VLAN, trong đó một cổng duy nhất trên router được cấu hình để xử lý nhiều VLAN thông qua các VLAN subinterfaces.
- Cách thức hoạt động:
  - Một router có một kết nối duy nhất đến switch, và trên router, mỗi VLAN sẽ được ánh xạ vào một subinterface với một địa chỉ IP khác nhau.
  - Switch sẽ truyền các gói dữ liệu từ một VLAN đến router thông qua một trunk port (cổng trunk), và router sẽ xử lý các gói đó dựa trên thẻ VLAN (VLAN tags).
  - Router sẽ định tuyến các gói giữa các VLAN, và gửi trả lại các gói đã được định tuyến tới các cổng của switch.
### Định tuyến giữa các VLAN bằng Layer 3 Switch (SVI - Switch Virtual Interface)
- Layer 3 Switch (hoặc Multilayer Switch) là một switch có khả năng thực hiện các chức năng của router, bao gồm định tuyến giữa các VLAN. Thay vì sử dụng một router để định tuyến, một switch Layer 3 có thể thực hiện trực tiếp việc định tuyến giữa các VLAN thông qua các SVI (Switch Virtual Interfaces).
- Cách thức hoạt động:
  - Trên switch Layer 3, bạn tạo ra các SVI cho từng VLAN. Mỗi SVI có một địa chỉ IP duy nhất và chức năng tương tự như một cổng giao tiếp mạng cho VLAN.
  - Switch Layer 3 sẽ thực hiện định tuyến nội bộ giữa các VLAN thông qua các SVI này mà không cần phải truyền lưu lượng tới một router bên ngoài.
