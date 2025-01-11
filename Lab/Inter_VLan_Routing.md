Mô Hình : ![image](https://github.com/user-attachments/assets/3d1b485d-7940-46fd-86b3-a42929892353)
### Cấu hình cổng trunk giữa các router
```
Sw1(config)#int e0/0
Sw1(config-if)#switchport trunk encapsulation dot1q
Sw1(config-if)#switchport mode trunk

Sw2(config)#int e0/1
Sw2(config-if)#switchport trunk encapsulation dot1q
Sw2(config-if)#sw mode trunk 
Sw2(config-if)#int e0/2
Sw2(config-if)#switchport trunk encapsulation dot1q
Sw2(config-if)#switchport mode trunk 

Sw3(config)#int e0/0
Sw3(config-if)#switchport trunk encapsulation dot1q 
Sw3(config-if)#switchport mode trunk
```
### Cấu hình Sw1 làm VTP server, Sw2 mode transparent và Sw3 mode client
```
Sw1(config)#vtp domain MUMEI
Changing VTP domain name from NULL to MUMEI
Sw1(config)#vtp mode server 
Device mode already VTP Server for VLANS.

Sw2(config)#vtp mode transparent 
Setting device to VTP Transparent mode for VLANS.

Sw3(config)#vtp mode client 
Setting device to VTP Client mode for VLANS.
Sw3(config)#vtp domain MUMEI
Changing VTP domain name from NULL to MUMEI
```
### Thêm Vlan ENG và VLan SALES
```
Sw1(config)#Vlan 10
Sw1(config-vlan)#name ENG
Sw1(config-vlan)#vlan 20
Sw1(config-vlan)#name SALES

Sw2(config)#Vlan 10
Sw2(config-vlan)#name ENG
Sw2(config-vlan)#vlan 20
Sw2(config-vlan)#name SALES
```
### Cấu hình cổng access giữa sw và pc
```
Sw1(config)#int ran
Sw1(config)#int range e0/1-2
Sw1(config-if-range)#switchport mode access 
Sw1(config-if-range)#switchport access vlan 10
Sw1(config-if-range)#int e0/3
Sw1(config-if)#switchport mode access 
Sw1(config-if)#switchport access vlan 20

Sw3(config)#int range e0/1-2
Sw3(config-if-range)#switchport mode access 
Sw3(config-if-range)#switchport access vlan 20
Sw3(config-if-range)#int e0/3
Sw3(config-if)#switchport mode access 
Sw3(config-if)#switchport access vlan 10
```
# Định tuyến Vlan
## Cách 1: Router on a stick
### Cấu hình thông Sw2 với Router
```
Sw2(config)#int e0/0
Sw2(config-if)#switchport trunk encapsulation dot1q 
Sw2(config-if)#switchport mode trunk 
Sw2(config-if)#switchport trunk allowed vlan 10,20 
```
### Cấu hình sub-interface trên router
```
R1(config)#int e0/0
R1(config-if)#no shutdown  
R1(config-if)#int e0/0.10
1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 10.10.10.1 255.255.255.0
R1(config-subif)#no shutdown 
R1(config-subif)#int e0/0.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 10.10.20.1 255.255.255.0   
R1(config-subif)#no shutdown 
```
### Ping từ pc ENG1 đến SALES1
```
ENG1> ping 10.10.20.10

84 bytes from 10.10.20.10 icmp_seq=1 ttl=63 time=3.299 ms
84 bytes from 10.10.20.10 icmp_seq=2 ttl=63 time=2.078 ms
84 bytes from 10.10.20.10 icmp_seq=3 ttl=63 time=1.968 ms
84 bytes from 10.10.20.10 icmp_seq=4 ttl=63 time=1.918 ms
84 bytes from 10.10.20.10 icmp_seq=5 ttl=63 time=1.442 ms
```
## Cách 2: Sử dụng Swich layer 3
Bật chức năng định tuyến trên sw layer3 ```Sw2(config)#ip routing```
### Cấu hình SVIs trên Sw2
```
Sw2(config)#interface vlan 10
Sw2(config-if)#ip address 10.10.10.1 255.255.255.0
Sw2(config-if)#no shutdown 
Sw2(config-if)#interface vlan 20                  
Sw2(config-if)#ip address 10.10.20.1 255.255.255.0
Sw2(config-if)#no shutdown
```
### Ping từ ENG1 đến SALES3 kiểm tra
```
ENG1> ping 10.10.20.12

84 bytes from 10.10.20.12 icmp_seq=1 ttl=63 time=0.975 ms
84 bytes from 10.10.20.12 icmp_seq=2 ttl=63 time=1.402 ms
84 bytes from 10.10.20.12 icmp_seq=3 ttl=63 time=1.032 ms
84 bytes from 10.10.20.12 icmp_seq=4 ttl=63 time=0.950 ms
84 bytes from 10.10.20.12 icmp_seq=5 ttl=63 time=1.011 ms
```

