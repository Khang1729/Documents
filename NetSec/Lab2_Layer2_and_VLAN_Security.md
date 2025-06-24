# Laboratory 2 - Layer 2 và VLAN-Security
## I. Giới thiệu
- Cấu hình VLAN, trunk.
- Gán cổng VLAN cho thiết bị đầu cuối (VPCS).
- Cấu hình bảo mật Layer 2: Port Security, DHCP Snooping.
- Kiểm tra bằng lệnh ping từ VPCS
## II. Công cụ
- Cisco IOSvL2: [https://drive.google.com/file/d/1QT7dqDeaQsTnzM7jD-_9ucr1p6mxLZh0/view](https://drive.google.com/file/d/1QT7dqDeaQsTnzM7jD-_9ucr1p6mxLZh0/view)
## III. Cài đặt công cụ:
Cấu hình Cisco IOSvL2
- Mở GNS3 -> Edit -> References ... -> Qemu VMs -> New -> Run this Qemu VM on the GNS3 VM -> đặt tên bắt kỳ cho Switch ảo -> qemu-system-x86_64 -> Telnet -> New Image -> chọn đường dẫn đến file IOSvL2 tải về -> Finish
## IV. Mô hình mạng thực hành:
![Model](Images/model_lab2.png)
## V. Nội dung thực hành
1. Cấu hình Switch IOSvL2 (SW1)
```
enable
conf t

! Tạo VLANs
vlan 10
 name SALES
vlan 20
 name IT

! Gán cổng cho VPCS
interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 20

! Cổng kết nối Router (Router-on-a-Stick)
interface FastEthernet0/24
 switchport trunk encapsulation dot1q
 switchport mode trunk
```
2. Cấu hình IP trên VPCS
- VPCS1 (VLAN 10): `ip 192.168.10.10/24 192.168.10.254`
- VPCS2 (VLAN 20): `ip 192.168.20.10/24 192.168.20.254`
3. Cấu hình Router:
```
enable
conf t

! Subinterface cho VLAN 10
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

! Subinterface cho VLAN 20
interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

! Bật cổng chính
interface GigabitEthernet0/0
 no shutdown
```
4. Kiểm tra kết nối (ping 2 VPCs được với nhau)
5. Thêm bảo mật Layer 2
- Cấu hình trên switch IOSvL2:
```
conf t

interface FastEthernet0/1
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky

interface FastEthernet0/2
 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```
- Giải thích:
+ maximum 1: chỉ cho 1 MAC trên mỗi cổng.
+ violation restrict: khi có MAC vi phạm, chặn truy cập, ghi log, không tắt cổng.
+ mac-address sticky: switch tự học địa chỉ MAC đầu tiên và lưu lại.

- Kiểm tra:`show port-security interface Fa0/1`
- Tình huống thử nghiệm (đơn giản):
+ Ngắt kết nối VPCS1 khỏi Fa0/1 → Kết nối lại VPCS3 vào Fa0/1.
+ Nếu VPCS3 có MAC khác → switch sẽ chặn, không cho vào mạng.

