# Cisco DHCP Server
Mô hình:

![image](https://github.com/user-attachments/assets/2d1288df-3bd3-4e38-89f9-3432934e8c28)
```
R1(config)#int e0/0
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#exit
R1(config)#ip dhcp pool MY_POOL
R1(dhcp-config)#network 192.168.1.0 255.255.255.0
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#dns-server 8.8.8.8
R1(dhcp-config)#exit
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10 #loai tru
R1(config)#end                
```
```
VPCS> ip dhcp
DDORA IP 192.168.1.11/24 GW 192.168.1.1

VPCS> show ip

NAME        : VPCS[1]
IP/MASK     : 192.168.1.11/24
GATEWAY     : 192.168.1.1
DNS         : 8.8.8.8  
DHCP SERVER : 192.168.1.1
DHCP LEASE  : 86390, 86400/43200/75600
MAC         : 00:50:79:66:68:02
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
```
# DHCP Relay Agent
Mô hình: ![image](https://github.com/user-attachments/assets/9f1aa480-ad33-4a97-afd2-7e77c75e21a9)
## Cấu hình DHCP server tương tự lưu ý DHCP server cần static route xuống pc
## Cấu Hình Relay Agent
```
R1(config)#int e0/0 #cổng trỏ xuống pc
R1(config-if)#ip helper-address 10.1.2.2 #địa chỉ dhcp server
```
