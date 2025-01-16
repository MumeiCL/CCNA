Mô hình:

![image](https://github.com/user-attachments/assets/c482e271-1561-4881-8b94-027bd0fb01c7)
# Numbered Standard ACL
### Cấu hình ACL trên R1 để chặn lưu lượng từ mạng con 10.0.2.0/24 đến R2. Các PC trong mạng con 10.0.1.0/24 và 10.0.2.0/24 phải duy trì kết nối với nhau. Các PC trong mạng con 10.0.1.0/24 phải duy trì kết nối với R2.
```
R1(config)#access-list 1 permit 10.0.1.0 0.0.0.255
R1(config)#access-list 1 deny 10.0.2.0 0.0.0.255
R1(config)#int e0/0
R1(config-if)#ip access-group 1 out
```
# Numbered Extended ACL
### Cấu hình ACL cho phép kết nối telnet chỉ từ PC1 đến R2, kết nối khác bt
```
R1(config)#access-list 100 permit tcp host 10.0.1.10 host 10.0.0.2 eq telnet 
R1(config)#access-lis 100 deny tcp 10.0.1.0 0.0.0.255 host 10.0.0.2 eq telnet      
R1(config)#access-list 100 permit ip any any
R1(config)#int e0/1                                   
R1(config-if)#ip access-group 100 in
```
# Named Extended ACL
### Cấu hình ACL cho phép kết nối telnet chỉ từ PC1 đến R2, ping chỉ từ PC2 đến R2, kết nối khác bt
```
R1(config)#ip access-list extended e0/1_in
R1(config-ext-nacl)#permit tcp host 10.0.1.10 host 10.0.0.2 eq telnet
R1(config-ext-nacl)#deny tcp 10.0.1.0 0.0.0.255 host 10.0.0.2 eq telnet
R1(config-ext-nacl)#permit icmp host 10.0.1.11 host 10.0.0.2 echo
R1(config-ext-nacl)#deny icmp 10.0.1.0 0.0.0.255 host 10.0.0.2 echo
R1(config-ext-nacl)#permit ip any any 
R1(config-ext-nacl)#end
R1#conf t
R1(config)#int e0/1
R1(config-if)#no ip access-group 100 in
R1(config-if)#ip access-group e0/1_in in
```
