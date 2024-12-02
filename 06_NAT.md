# NAT (Network Address Translation)
- là một cơ chế trong mạng máy tính cho phép thay đổi địa chỉ IP trong các gói tin khi chúng đi qua thiết bị mạng, chẳng hạn như router hoặc firewall. NAT thường được sử dụng để ánh xạ giữa các địa chỉ IP riêng (private IP) và địa chỉ IP công cộng (public IP).
## Lợi ích
- Tiết kiệm địa chỉ IP công cộng: Cho phép nhiều thiết bị trong mạng riêng chia sẻ một hoặc một số ít địa chỉ IP công cộng.
- Tăng cường bảo mật: Ẩn các địa chỉ IP thật của thiết bị trong mạng nội bộ.
- Linh hoạt trong quản lý mạng: Giúp triển khai các thiết bị trong mạng riêng dễ dàng hơn. hi bạn sử dụng địa chỉ Ip private dù cho bạn có đổi nhà cung cấp dịch vụ, bạn sẽ không cần phải đánh lại địa chỉ cho các thiết bị trong mạng cục bộ mà bạn chỉ phải thay đổi cấu hình NAT trên firewall để trùng với địa chỉ IP public mới.
## Phân loại các kỹ thuật NAT
### Static NAT
- Static NAT được sử dụng để chuyển đổi một địa chỉ IP này sang một địa chỉ khác một cách cố định, thường là từ một địa chỉ cục bộ (private IP) sang một địa chỉ công cộng (public IP). Quá trình này được cấu hình thủ công, nghĩa là một địa chỉ IP cục bộ sẽ ánh xạ với một địa chỉ IP công cộng một cách rõ ràng và duy nhất.
- Static NAT rất hữu ích trong các trường hợp cần thiết bị có địa chỉ cố định để có thể truy cập từ bên ngoài Internet. Những thiết bị này thường là các máy chủ như Web server, Mail server,...
### Dymanic NAT
- tự động ánh xạ địa chỉ IP nội bộ (private IP) sang địa chỉ IP công cộng (public IP) từ một nhóm địa chỉ IP công cộng được cấu hình sẵn. Không giống như Static NAT, quá trình ánh xạ này không cố định; địa chỉ IP công cộng được cấp phát tạm thời khi thiết bị trong mạng nội bộ gửi yêu cầu ra ngoài.
### NAT overload
- NAT Overload là một dạng của Dynamic NAT. Nó thực hiện ánh xạ nhiều địa chỉ IP nội bộ thành một địa chỉ IP công cộng (mô hình many-to-one) và sử dụng các số cổng khác nhau để phân biệt từng kết nối. NAT Overload còn được gọi là PAT (Port Address Translation).Chỉ số cổng được mã hóa 16 bit, vì vậy có tới 65.536 địa chỉ nội bộ có thể được ánh xạ thông qua một địa chỉ IP công cộng duy nhất.
