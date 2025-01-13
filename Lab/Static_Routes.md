Mô hình:![image](https://github.com/user-attachments/assets/4d8144e6-814b-4842-b4b9-52bb3f42d43e)
## Static Routes
### Cấu hình static routes trên R1, R2, R3, R4
```
R1(config)#ip route 10.1.0.0 255.255.255.0 10.0.0.2
R1(config)#ip route 10.1.1.0 255.255.255.0 10.0.0.2
R1(config)#ip route 10.1.2.0 255.255.255.0 10.0.0.2
R1(config)#ip route 10.1.3.0 255.255.255.0 10.0.0.2

R2(config)#ip route 10.0.1.0 255.255.255.0 10.0.0.1
R2(config)#ip route 10.0.2.0 255.255.255.0 10.0.0.1
R2(config)#ip route 10.0.3.0 255.255.255.0 10.0.0.1
R2(config)#ip route 10.1.1.0 255.255.255.0 10.1.0.1
R2(config)#ip route 10.1.2.0 255.255.255.0 10.1.0.1
R2(config)#ip route 10.1.3.0 255.255.255.0 10.1.0.1

R3(config)#ip route 10.0.0.0 255.255.255.0 10.1.0.2
R3(config)#ip route 10.0.1.0 255.255.255.0 10.1.0.2
R3(config)#ip route 10.0.2.0 255.255.255.0 10.1.0.2
R3(config)#ip route 10.0.3.0 255.255.255.0 10.1.0.2
R3(config)#ip route 10.1.2.0 255.255.255.0 10.1.1.1
R3(config)#ip route 10.1.3.0 255.255.255.0 10.1.1.1

R4(config)#ip route 10.1.0.0 255.255.255.0 10.1.1.2
R4(config)#ip route 10.0.0.0 255.255.255.0 10.1.1.2
R4(config)#ip route 10.0.1.0 255.255.255.0 10.1.1.2
R4(config)#ip route 10.0.2.0 255.255.255.0 10.1.1.2
R4(config)#ip route 10.0.3.0 255.255.255.0 10.1.1.2
```
### Từ PC3 ping đến PC1, PC2 để kiểm tra
```
PC3> ping 10.0.1.10

84 bytes from 10.0.1.10 icmp_seq=1 ttl=60 time=2.692 ms
84 bytes from 10.0.1.10 icmp_seq=2 ttl=60 time=2.588 ms
84 bytes from 10.0.1.10 icmp_seq=3 ttl=60 time=2.139 ms
84 bytes from 10.0.1.10 icmp_seq=4 ttl=60 time=2.024 ms
84 bytes from 10.0.1.10 icmp_seq=5 ttl=60 time=1.695 ms

PC3> ping 10.0.2.10

84 bytes from 10.0.2.10 icmp_seq=1 ttl=60 time=2.459 ms
84 bytes from 10.0.2.10 icmp_seq=2 ttl=60 time=2.243 ms
84 bytes from 10.0.2.10 icmp_seq=3 ttl=60 time=1.886 ms
84 bytes from 10.0.2.10 icmp_seq=4 ttl=60 time=2.050 ms
84 bytes from 10.0.2.10 icmp_seq=5 ttl=60 time=2.001 ms
```
### Theo dõi chi tiết đường đi từ PC3 đến PC1
```
PC3> trace 10.0.1.10   
1   10.1.2.1   0.611 ms  
2   10.1.1.2   1.599 ms  
3   10.1.0.2   1.810 ms  
4   10.0.0.1   1.627 ms  
5   10.0.1.10  4.307 ms
```
## Summary Routes
### Xoá hết lệnh static route trên R1
```
R1(config)#no ip route 10.1.0.0 255.255.255.0 10.0.0.2
R1(config)#no ip route 10.1.1.0 255.255.255.0 10.0.0.2
R1(config)#no ip route 10.1.2.0 255.255.255.0 10.0.0.2
R1(config)#no ip route 10.1.3.0 255.255.255.0 10.0.0.2
```
### Summary route
```
R1(config)#ip route 10.1.0.0 255.255.0.0 10.0.0.2
```
### Kiểm tra kết nối từ PC1 đến PC3
```
PC1> ping 10.1.2.10

84 bytes from 10.1.2.10 icmp_seq=1 ttl=60 time=4.235 ms
84 bytes from 10.1.2.10 icmp_seq=2 ttl=60 time=2.600 ms
84 bytes from 10.1.2.10 icmp_seq=3 ttl=60 time=2.397 ms
84 bytes from 10.1.2.10 icmp_seq=4 ttl=60 time=1.551 ms
84 bytes from 10.1.2.10 icmp_seq=5 ttl=60 time=1.367 ms

PC1> trace 10.1.2.10
trace to 10.1.2.10, 8 hops max, press Ctrl+C to stop
 1   10.0.1.1   1.107 ms  
 2   10.0.0.2   0.469 ms  
 3   10.1.0.1   0.698 ms 
 4   10.1.1.1   1.383 ms  
 5   10.1.2.10  1.820 ms 
```
## Longest Prefix Match
### Cấu hình summary route cho R5 kết nối đến R1
```
R5(config)#ip route 10.0.0.0 255.255.0.0 10.0.3.1
```
### Kiểm tra đường đi từ PC1 đến R5
```
PC1> trace 10.1.3.2
trace to 10.1.3.2, 8 hops max, press Ctrl+C to stop
 1   10.0.1.1   0.591 ms  
 2   10.0.0.2   0.580 ms 
 3   10.1.0.1   1.353 ms  
 4   10.1.1.1   1.542 ms  
 5   10.1.3.2   0.820 ms
```
Thứ tự đi lần lượt R1 > R2 > R3 > R4 > R5
```
R5#traceroute 10.0.1.10
1 10.0.3.1 1 msec 1 msec 0 msec
2 10.0.1.10 3 msec 1 msec 1 msec
```
Thứ tự đi lần lượt R5 > R1
```
R1(config)#ip route 10.1.3.0 255.255.255.0 10.0.3.2
S        10.1.0.0/16 [1/0] via 10.0.0.2
S        10.1.3.0/24 [1/0] via 10.0.3.2
```
Tuyến đường khớp với tiền tố (prefix) dài nhất phù hợp với địa chỉ IP đích sẽ được chọn.


