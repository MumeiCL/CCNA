# Access Control Lists
## Tổng quát
- ACL là một danh sách các điều kiện được áp đặt vào các cổng của router để lọc các gói tin đi qua nó. Danh sách này chỉ ra cho router biết loại dữ liệu nào được phép (allow) và loại dữ liệu nào bị hủy bỏ (deny). Sự cho phép và hủy bỏ này có thể được kiểm tra dựa vào địa chỉ nguồn, địa chỉ đích, giao thức hoặc chỉ số cổng.
- Ưu điểm:
  - Dễ cấu hình, áp dụng nhanh chóng.
  - Tăng cường bảo mật mạng.
  - Giảm thiểu nguy cơ truy cập trái phép.
- Nhược điểm:
  - Không phù hợp với mạng lớn, phức tạp.
  - Khó quản lý khi có quá nhiều quy tắc.
  - Có thể ảnh hưởng hiệu suất khi ACL phức tạp.
### Một số khái niệm:
- Wildcard mask
"Wildcard mask" có 32 bit, chia thành 4 phần, mỗi phần có 8 bit, là tham số được dùng xác định các bit nào sẽ được bỏ qua hay buộc phải so trùng trong việc kiểm tra điều kiện. Bit '1' trong "wildcard mask" có nghĩa là bỏ qua vị trí bit đó khi so sánh, và bit '0' xác định vị trí bit đó phải giống nhau.

  Với Standard ACL, nếu không thêm "wildcard-mask" trong câu lệnh tạo ACL thì mặc định "wildcard-mask" sẽ là 0.0.0.0.

  Mặc dù "Wildcard mask" có cấu trúc 32 bit giống với "Subnet mask" nhưng chúng hoạt động khác nhau. Các bit 0 và 1 trong một "Subnet mask" xác định phần "Network" và phần "Host" trong một địa chỉ IP. Các bit 0 và 1 trong một "wildcard-mask" xác định bit nào sẽ được kiểm tra hay bỏ qua cho mục đích điều khiển truy cập.
- Wildcard Host Chỉ định một địa chỉ IP duy nhất. Mặc định là 0.0.0.0, nghĩa là kiểm tra toàn bộ các bit trong địa chỉ IP. Dùng khi cần cho phép hoặc từ chối một địa chỉ IP cụ thể.

  `access-list 10 permit 192.168.1.10 0.0.0.0` chỉ địa chỉ 192.168.1.10 được áp dụng.
- Wildcard Any Chỉ định tất cả các địa chỉ IP. Mặc định là 255.255.255.255, nghĩa là bỏ qua toàn bộ các bit trong địa chỉ IP khi kiểm tra. Dùng khi cần áp dụng cho tất cả các địa chỉ IP.

  `access-list 10 permit 0.0.0.0 255.255.255.255` cho phép tất cả các địa chỉ IP.
- Inbound: ACL kiểm tra các gói tin trước khi chúng được định tuyến (routing). Gói tin đến một giao diện (interface) sẽ được kiểm tra và xử lý theo quy tắc ACL ngay tại điểm vào. Nếu bị từ chối, gói tin sẽ không được tiếp tục xử lý.\
- Outbound: ACL kiểm tra các gói tin sau khi chúng được định tuyến (routing). Gói tin sẽ được định tuyến đến giao diện đầu ra (interface) và kiểm tra tại đây trước khi rời khỏi thiết bị.
### Standard ACL
- là một danh sách kiểm soát truy cập đơn giản được sử dụng để kiểm soát lưu lượng mạng dựa trên địa chỉ IP nguồn. Nó hoạt động như một bộ lọc, cho phép hoặc từ chối các gói tin dựa trên một quy tắc đơn giản: cho phép hoặc từ chối tất cả các gói tin đến từ một mạng cụ thể.
### Extended ACL
- là một danh sách kiểm soát truy cập nâng cao, cung cấp khả năng lọc lưu lượng mạng chi tiết hơn so với Standard ACL. Nếu Standard ACL chỉ kiểm soát dựa trên địa chỉ IP nguồn, thì Extended ACL cho phép bạn kiểm soát dựa trên nhiều tiêu chí hơn, bao gồm:
  - Địa chỉ IP nguồn: Giống như Standard ACL
  - Địa chỉ IP đích: Chỉ định mạng đích mà bạn muốn cho phép hoặc chặn.
  - Số cổng: Xác định các cổng giao thức (ví dụ: cổng 22 cho SSH, cổng 80 cho HTTP).
  - Giao thức: Chỉ định giao thức mạng (ví dụ: TCP, UDP, ICMP).
### Named ACL và Numbered ACL
- Numbered ACL: không cho phép chèn, sửa , xóa trên từng dòng mà phải viết lại ACL nếu sai sót.
- Named ACL: cho phép, chèn, sửa, xóa từng dòng.
