Laboratory 3 - AAA, DHCP, NAT, Wireless
## I. Giới thiệu
- Hiểu và cấu hình mô hình xác thực AAA.
- Triển khai máy chủ DHCP và cấu hình cấp phát IP động.
- Cấu hình NAT (Static, Dynamic, PAT) trên router.
- Mô phỏng mạng không dây với bảo mật WPA2/WPA3.
## III. Cài đặt công cụ: 
Sử dụng các công cụ thực hành trước
## IV. Mô hình mạng thực hành:
![Model](Images/model_lab3.png)
## V. Nội dung thực hành
1. Cấu hình AAA (Authentication, Authorization, Accounting)
```
conf t
aaa new-model
aaa authentication login default local
username admin secret 123456
```
Bật xác thực cho console và telnet/SSH:
```
line console 0
 login authentication default

line vty 0 4
 login authentication default
 transport input telnet ssh
```
Kết quả: khi truy cập console hoặc Telnet → yêu cầu user/pass (admin/123456)
2. Cấu hình DHCP trên Router
```
conf t
ip dhcp excluded-address 192.168.1.1 192.168.1.10

ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
```
Đảm bảo interface LAN đã có IP:
```
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```
3. Cấu hình NAT (Internet giả định)
Mô hình NAT:
- Inside (LAN): 192.168.1.0/24
- Outside (Internet): Giả lập 10.0.0.1/24
```
interface GigabitEthernet0/0
 ip nat inside

interface GigabitEthernet0/1
 ip address 10.0.0.1 255.255.255.0
 ip nat outside
```
Cấu hình NAT overload:
```
access-list 1 permit 192.168.1.0 0.0.0.255

ip nat inside source list 1 interface GigabitEthernet0/1 overload
```
NAT overload cho phép nhiều IP nội bộ chia sẻ 1 IP ra ngoài.

4. Cấu hình Wireless (mô phỏng)
- Vì GNS3 không có thiết bị AP nên dùng Switch mô phỏng có VLAN riêng cho Wi-Fi → tạo như mạng LAN bình thường.
```
vlan 20
 name WIFI

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20
```
- Trên router, tạo subinterface cho VLAN 20:
```
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
```
- Cấu hình DHCP riêng cho VLAN 20:
```
ip dhcp pool WIFI
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
```
