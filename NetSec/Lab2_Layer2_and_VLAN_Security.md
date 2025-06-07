# Laboratory 1 - Layer 2 và VLAN-Security
## I. Giới thiệu
- Hiểu cách hoạt động của VLAN và vai trò của phân tách miền quảng bá (broadcast domains).
- Thực hành cấu hình VLAN, VLAN Trunking.
- Nhận diện các lỗ hổng Layer 2 (VLAN Hopping, MAC Flooding, DHCP Spoofing,...).
- Cấu hình các cơ chế bảo mật cấp Layer 2 như: Port Security, BPDU Guard, DHCP Snooping, Dynamic ARP Inspection (DAI).
## II. Yêu cầu công cụ
- Wireshark: [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
- Yersinia: [https://www.kali.org/tools/yersinia/](https://www.kali.org/tools/yersinia/)
## III. Nội dung thực hành
1. Cấu hình VLAN và Trunk
Trên Switch:


Tạo VLAN:

 pgsql
CopyEdit
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

