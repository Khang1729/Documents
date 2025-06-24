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
2. Cấu hình VLAN và Trunk
Trên Switch:


Tạo VLAN:

vlan 10
name SALES
vlan 20
name IT


Gán cổng cho VLAN:

 kotlin
CopyEdit
interface FastEthernet0/1
switchport mode access
switchport access vlan 10

interface FastEthernet0/2
switchport mode access
switchport access vlan 20


Cấu hình trunk giữa switch/router:

 kotlin
CopyEdit
interface FastEthernet0/24
switchport mode trunk


Trên Router (Router-on-a-Stick hoặc L3 Switch):


Cấu hình subinterface:

 nginx
CopyEdit
interface Gig0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface Gig0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0


Kiểm tra:


Ping giữa các máy trong cùng VLAN và khác VLAN.



Phần B: Thực hành tấn công và phòng thủ Layer 2
B1. VLAN Hopping (Double Tagging)
Thực hiện (trên Ubuntu cài yersinia):

 bash
CopyEdit
sudo yersinia -G


Tạo gói tấn công double-tag VLAN.


Phòng thủ:


Trên Switch, tắt trunk không cần thiết và gán native VLAN không sử dụng:

 java
CopyEdit
switchport trunk allowed vlan 10,20
switchport trunk native vlan 999
vlan 999



B2. MAC Flooding
Thực hiện (trên máy tấn công dùng macof):

 bash
CopyEdit
sudo apt install dsniff
sudo macof -i eth0


Phòng thủ – Port Security:

 pgsql
CopyEdit
interface FastEthernet0/1
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
switchport port-security mac-address sticky



B3. DHCP Spoofing
Tấn công bằng dhcpstarv và rogue dhcp tools.


Phòng thủ – DHCP Snooping:

 kotlin
CopyEdit
ip dhcp snooping
ip dhcp snooping vlan 10,20
interface FastEthernet0/24
ip dhcp snooping trust



B4. ARP Spoofing / MITM
Dùng arpspoof để tấn công.


Phòng thủ – Dynamic ARP Inspection (DAI):

 kotlin
CopyEdit
ip arp inspection vlan 10,20
interface FastEthernet0/24
ip arp inspection trust

