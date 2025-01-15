Mô hình: ![image](https://github.com/user-attachments/assets/9057e5b6-e9ad-4bc6-95ba-d68bfc3cee15)
# Cấu hình cơ bản
```
R1(config)#int e0/1
R1(config-if)#standby 1 ip 10.10.10.1
R1(config-if)#standby 1 priority 110 
R1(config-if)#standby 1 preempt

R2(config)#int e0/1
R2(config-if)#standby 1 ip 10.10.10.1
R2(config-if)#standby 1 priority 90
```
Route có priority lớn hơn sẽ làm active, Preemption cho phép một router với độ ưu tiên cao hơn (higher priority) được quyền giành lại vai trò Active Router trong nhóm HSRP nếu nó hoạt động trở lại sau khi bị lỗi hoặc khởi động lại.
# load balancing với HSRP
## Option 1
```
R1(config)#int e0/1
R1(config-if)#standby 1 ip 10.10.10.1
R1(config-if)#standby 1 priority 110 
R1(config-if)#standby 1 preempt
R1(config-if)#standby 2 ip 10.10.10.254
R1(config-if)#standby 2 priority 90
```
```
R2(config)#int e0/1
R2(config-if)#standby 1 ip 10.10.10.1
R2(config-if)#standby 1 priority 90
R1(config-if)#standby 2 ip 10.10.10.254
R1(config-if)#standby 2 priority 110 
R1(config-if)#standby 2 preempt
```
50% pc sẽ sử dụng ip 10.10.10.1 làm default gateway, 50% còn lại sử dụng ip 10.10.10.254 làm default gateway
## Option 2
```
R1(config)#int e0/1
R1(config-if)#standby 1 ip 10.10.10.1
R1(config-if)#standby 1 priority 110 
R1(config-if)#standby 1 preempt
R1(config)#int e0/2
R1(config-if)#standby 1 ip 10.10.20.1
R1(config-if)#standby 1 priority 90 
```
```
R2(config)#int e0/1
R2(config-if)#standby 1 ip 10.10.10.1
R2(config-if)#standby 1 priority 90
R2(config)#int e0/2
R2(config-if)#standby 1 ip 10.10.20.1
R2(config-if)#standby 1 priority 110 
R2(config-if)#standby 1 preempt
```
R1 là HSRP active cho ip 10.10.10.1, R2 cho ip 10.10.20.1
