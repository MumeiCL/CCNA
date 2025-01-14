# Single area OSPF
Mô hình: ![image](https://github.com/user-attachments/assets/96d71439-ed1c-4e93-bfd7-b8aa8065eb07)
## Cấu hình interface loopback trên mỗi router
192.168.0.x với x là số thứ tự Route
```
R1(config)#interface loopback0
R1(config-if)#ip address 192.168.0.1 255.255.255.255
```
## Kích hoạt OSPF trên mỗi Route
```
R1(config)#router ospf 1
R1(config-router)#network 10.0.0.0 0.255.255.255 area 0
R1(config-router)#network 192.168.0.0 0.0.0.255 area 0
```
## OSPF Cost
Cost= Interface Bandwidth/Reference Bandwidth

### Thay đổi giá trị reference-bandwidth trên mỗi route
```
R1(config)#router ospf 1
R1(config-router)#auto-cost reference-bandwidth 10000
```
### Có 2 đường đi từ R1 đến 10.1.2.0/24 từ R1 > R2 và R1 > R5 nhưng trong routing table chỉ hiện quãng đường từ R1 > R5
```
R1#show ip route
      10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
C        10.0.0.0/24 is directly connected, Ethernet0/0
L        10.0.0.1/32 is directly connected, Ethernet0/0
C        10.0.3.0/24 is directly connected, Ethernet0/1
L        10.0.3.1/32 is directly connected, Ethernet0/1
O        10.1.0.0/24 [110/2000] via 10.0.0.2, 00:00:35, Ethernet0/0
O        10.1.1.0/24 [110/3000] via 10.0.3.2, 00:00:25, Ethernet0/1
                     [110/3000] via 10.0.0.2, 00:00:35, Ethernet0/0
O        10.1.2.0/24 [110/3000] via 10.0.3.2, 00:00:25, Ethernet0/1
O        10.1.3.0/24 [110/2000] via 10.0.3.2, 00:00:25, Ethernet0/1
      192.168.0.0/32 is subnetted, 4 subnets
C        192.168.0.1 is directly connected, Loopback0
O        192.168.0.3 [110/2001] via 10.0.0.2, 00:00:35, Ethernet0/0
O        192.168.0.4 [110/2001] via 10.0.3.2, 00:00:25, Ethernet0/1
O        192.168.0.5 [110/1001] via 10.0.3.2, 00:01:08, Ethernet0/1
```
### Mỗi interface đều có cost = 1000, R1 > R5 > R4 có cost = 3000 trong khi R1 > R2 > R3 > R4 có cost = 4000 để cấu hình 2 quãng đường có cost bằng nhau phục vụ loadbalanced cần thay đổi cost R1 > R5 và R5 > R4 thành 1500
```
R1(config)#int e0/1
R1(config-if)#ip ospf cost 1500

R4(config)#int e0/1
R4(config-if)#ip ospf cost 1500

R5(config)#int e0/1
R5(config-if)#ip ospf cost 1500
R5(config-if)#int e0/0
R5(config-if)#ip ospf cost 1500
```
Kiểm tra lại cost 2 quãng đường đã bằng nhau 
```
R1#show ip route 
      10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
C        10.0.0.0/24 is directly connected, Ethernet0/0
L        10.0.0.1/32 is directly connected, Ethernet0/0
C        10.0.3.0/24 is directly connected, Ethernet0/1
L        10.0.3.1/32 is directly connected, Ethernet0/1
O        10.1.0.0/24 [110/2000] via 10.0.0.2, 00:11:42, Ethernet0/0
O        10.1.1.0/24 [110/3000] via 10.0.0.2, 00:11:42, Ethernet0/0
O        10.1.2.0/24 [110/4000] via 10.0.3.2, 00:00:33, Ethernet0/1
                     [110/4000] via 10.0.0.2, 00:00:33, Ethernet0/0
O        10.1.3.0/24 [110/3000] via 10.0.3.2, 00:00:33, Ethernet0/1
      192.168.0.0/32 is subnetted, 4 subnets
C        192.168.0.1 is directly connected, Loopback0
O        192.168.0.3 [110/2001] via 10.0.0.2, 00:11:42, Ethernet0/0
O        192.168.0.4 [110/3001] via 10.0.3.2, 00:00:33, Ethernet0/1
                     [110/3001] via 10.0.0.2, 00:00:33, Ethernet0/0
```
# Multi-Area OSPF
Mô hình: ![image](https://github.com/user-attachments/assets/7301b3f3-5345-4b38-ba9d-9b9c8237313a)
## Giữ nguyên cấu hình trên R3,4 thay đổi cấu hình R1,2,5 
```
R1(config)#router ospf 1
R1(config-router)#network 10.0.0.0 0.255.255.255 area 1
R1(config-router)#network 192.168.0.0 0.0.0.255 area 1

R2(config)#router ospf 1
R2(config-router)#no network 10.0.0.0 0.255.255.255 area 0
R2(config-router)#network 10.1.0.0 0.0.0.255 area 0
R2(config-router)#network 10.0.0.0 0.0.0.255 area 1

R5(config)#router ospf 1
R5(config-router)#no network 10.0.0.0 0.255.255.255 area 0
R5(config-router)#network 10.1.3.0 0.0.0.255 area 0
R5(config-router)#network 10.0.3.0 0.0.0.255 area 1
```
## Cấu hình Summary Route trên các Area Border giúp giảm số lượng route được quảng bá qua các khu vực OSPF Tối ưu hóa bảng định tuyến, tiết kiệm băng thông
```
R2(config)#router ospf 1
R2(config-router)#area 0 range 10.1.0.0 255.255.0.0
R2(config-router)#area 1 range 10.0.0.0 255.255.0.0

R5(config)#router ospf 1
R5(config-router)#area 0 range 10.1.0.0 255.255.0.0
R5(config-router)#area 1 range 10.0.0.0 255.255.0.0
```
# DR and BDR  Designated Routers
