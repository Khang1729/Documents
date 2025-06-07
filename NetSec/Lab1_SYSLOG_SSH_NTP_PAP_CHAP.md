# Laboratory 1 - SYSLOG, SSH, NTP, PAP, CHAP
## I. Giới thiệu
1. Ghi log tập trung (Syslog):
- Sử dụng để thu thập và lưu trữ thông tin hệ thống từ các thiết bị mạng một cách tập trung.
- Link tham khảo: [https://datatracker.ietf.org/doc/html/rfc5424](https://datatracker.ietf.org/doc/html/rfc5424)
2. Truy cập từ xa an toàn (SSH):
- Giao thức bảo mật cho phép truy cập dòng lệnh đến các thiết bị từ xa.
- Link tham khảo: [https://datatracker.ietf.org/doc/html/rfc4251](https://datatracker.ietf.org/doc/html/rfc4251)
3. Đồng bộ thời gian mạng (NTP):
- Giao thức giúp đồng bộ hóa thời gian hệ thống giữa các thiết bị trên mạng.
- Link tham khảo: [https://datatracker.ietf.org/doc/html/rfc5905](https://datatracker.ietf.org/doc/html/rfc5905)
4. Xác thực bằng PAP (Password Authentication Protocol):
- Một phương thức xác thực cơ bản qua mạng, thường dùng trong PPP.
- Link tham khảo: [https://datatracker.ietf.org/doc/html/rfc1334](https://datatracker.ietf.org/doc/html/rfc1334)
5. Xác thực bằng CHAP (Challenge-Handshake Authentication Protocol):
- Một giao thức xác thực bảo mật hơn PAP, sử dụng cơ chế thử thách-đáp ứng. 
- Link tham khảo: [https://datatracker.ietf.org/doc/html/rfc1994](https://datatracker.ietf.org/doc/html/rfc1994)
## II. Các công cụ cần thiết
- GNS3: [https://www.gns3.com/software/download](https://www.gns3.com/software/download)
- GNS3 VM: [https://www.gns3.com/software/download-vm](https://www.gns3.com/software/download-vm)
- Router IOS (Cisco IOS image): [https://drive.google.com/file/d/1nKteBUix69_-A0rxhxAovwmZt0O8Nq0v](https://drive.google.com/file/d/1nKteBUix69_-A0rxhxAovwmZt0O8Nq0v)
- VMware Workstation: [https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)
- Ubuntu Server: [https://ubuntu.com/download/server](https://ubuntu.com/download/server)
## III. Cài đặt công cụ và chuẩn bị thực hành
1. Cài VMWare Workstation: Mở file cài đặt .exe và làm theo các bước mặc định (cứ nhấn “Next” cho đến khi hoàn tất).
2. Tạo máy ảo và cài đặt Ubuntu Server
- Mở VMWare, chọn "Create a New Virtual Machine"
- Làm theo hướng dẫn để tạo máy ảo mới
- Chọn file cài Ubuntu Server (.iso) và cài đặt hệ điều hành lên máy ảo đó
3. Cài đặt GNS3: Mở file cài đặt .exe và làm theo các bước mặc định (cứ nhấn “Next” cho đến khi hoàn tất).
4. Cài đặt GNS3 VM: Mở VMware Workstation, vào File > Open... và chọn file .ova của GNS3 VM để nhập (import).
5. Cấu hình Router IOS:
- Mở GNS3 -> Run the appliance on my local computer
- Đặt tên dự án và đường dẫn lưu dự án
- Vào menu Edit -> Preferences ... -> IOS Routers -> New -> Chọn file .image -> (cứ nhấn "Next" cho đến khi hoàn tất) -> OK
6. Tạo Node Ubuntu Server for GNS3
- Vào menu Edit -> Preferences ... -> VMware VMs -> New -> Chọn Ubuntu Server -> Finish -> Ok
## IV. Nội dung thực hành
1. Cấu hình Ubuntu Server (Syslog Server)
- Cài đặt rsyslog (thường có sẵn) hoặc syslog-ng
```
sudo apt update
sudo apt install rsyslog
```
- Cấu hình rsyslog để nhận log từ xa:
  + Chỉnh sửa file cấu hình rsyslog: `sudo nano /etc/rsyslog.conf`
  + Khởi động lại dịch vụ rsyslog: `sudo systemctl restart rsyslog`
  + Tìm và bỏ ghi chú (uncomment) các dòng sau để cho phép nhận UDP và TCP (UDP là phổ biến hơn cho syslog):
```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
# module(load="imtcp")
# input(type="imtcp" port="514")
```
  + Thêm dòng sau vào cuối file để lưu log từ các thiết bị từ xa vào một file riêng (ví dụ: /var/log/cisco_router.log):
```
if $fromhost contains '192.168.100.1' then /var/log/cisco_router.log
& ~ # Không xử lý các log này bằng các quy tắc mặc định khác
```
  + Kiểm tra trạng thái cổng mở
```
sudo systemctl status rsyslog
sudo ufw allow 514/udp # Nếu UFW đang hoạt động
sudo ufw allow 514/tcp # Nếu dùng TCP
```
- Cấu hình Router trên GNS3 (Syslog Client)
```
Router#conf t
Router(config)logging <IP_syslog_server> # Địa chỉ IP của Ubuntu Server
Router(config)logging trap informational # Mức độ log gửi đi (0-7, informational = 6)
Router(config)logging source-interface FastEthernet0/0 # Sử dụng interface này làm nguồn khi gửi log
Router(config)logging on # Đảm bảo logging được bật (thường là mặc định)
```
Mức độ log:
0: emergencies
1: alerts
2: critical
3: errors
4: warnings
5: notifications
6: informational
7: debugging

- Kiểm tra Log nhận được trên Server
  + Trên Router thực hiện các hành động để tạo Log
```
Router#configure terminal
Router(config)#interface Loopback0
Router(config-if)#ip address 1.1.1.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#end
Router#exit
```
  + Trên Ubuntu Server kiểm tra file log: `tail -f /var/log/cisco_router.log`
2. Cấu hình SSH
- Trên Router Cisco:
  + Đặt hostname và domain-name: Cần thiết để tạo khóa mã hóa.
```
Router#configure terminal
Router(config)#hostname R1
R1(config)#ip domain-name lab.local
```
  + Tạo khóa mã hóa RSA:
```
R1(config)#crypto key generate rsa
How many bits in the modulus [512]: 1024 # Nên chọn 1024 hoặc 2048 để tăng cường bảo mật
```
  + Tạo người dùng local: `R1(config)#username admin secret Cisco123`
Lưu ý: secret mã hóa mật khẩu mạnh hơn password.
  + Cấu hình VTY lines cho SSH:
```
R1(config)#line vty 0 4
R1(config-line)#transport input ssh # Chỉ cho phép SSH, không cho phép Telnet
R1(config-line)#login local # Sử dụng cơ sở dữ liệu người dùng local
R1(config-line)#exit
```
  + Cấu hình SSH version (tùy chọn nhưng khuyến nghị): `R1(config)#ip ssh version 2 # Nên dùng SSHv2 để bảo mật hơn`
  + Kích hoạt SSH: `R1(config)#ip ssh enable # SSH sẽ tự động kích hoạt sau khi tạo khóa`

- Trên Máy tính Client kiểm tra kết nối SSH: `ssh admin@<IP_router>`
  openssh-client -y
3. Cấu hình NTP
- Cài đặt NTP Server trên Ubuntu Server:
  + Cài đặt NTP server (chrony hoặc ntp): chrony được khuyến nghị cho các phiên bản Ubuntu Server mới hơn.
```
sudo apt update
sudo apt install chrony -y
```
  + Chỉnh sửa file cấu hình chrony: `sudo nano /etc/chrony/chrony.conf `
  + Thêm dòng sau để cho phép Router R1 truy cập (thay 192.168.100.0/24 bằng dải mạng của bạn): `allow 192.168.100.0/24`
    Kiểm tra và sửa đổi các dòng pool hoặc server để chrony tự đồng bộ với các NTP server công cộng (ví dụ: pool ntp.ubuntu.com iburst). Lưu file và thoát.
  + Khởi động lại dịch vụ chrony: `sudo systemctl restart chrony`
  + Kiểm tra trạng thái dịch vụ và mở port 123 (nếu có tường lửa):
```
sudo systemctl status chrony
sudo ufw allow 123/udp # Nếu UFW đang hoạt động
```
- Cấu hình trên Router R1 (NTP Client)
  + Đặt múi giờ và thời gian (tùy chọn nhưng khuyến nghị):
```
R1(config)#clock timezone ICT 7
R1(config)#clock set 10:00:00 07 Jun 2025 # Cấu hình thời gian gần đúng trước khi đồng bộ
```
  - Cấu hình NTP Server:
```
R1(config)#ntp server 192.168.100.2 prefer # Địa chỉ IP của Ubuntu Server
Lưu ý: prefer ưu tiên server này nếu có nhiều server NTP.
```
- Kiểm tra NTP
Trên Router R1:
Kiểm tra trạng thái đồng bộ NTP: `R1#show ntp status`
Bạn sẽ thấy trạng thái "Clock is synchronized" hoặc tương tự.
Kiểm tra các mối quan hệ NTP: `R1#show ntp associations `
Bạn sẽ thấy Ubuntu Server được liệt kê là NTP peer.
Kiểm tra thời gian hệ thống:`R1#show clock detail `
Thời gian trên Router R1 sẽ được đồng bộ với Ubuntu Server.

4. Cấu hình PAP/CHAP
- Cấu hình trên R1:

R1(config)#interface FastEthernet0/1
R1(config-if)#ip address 10.0.0.1 255.255.255.252
R1(config-if)#no shutdown
R1(config-if)#encapsulation ppp # Đặt encapsulation là PPP
Cấu hình trên R2:

R2(config)#interface FastEthernet0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#no shutdown
R2(config-if)#encapsulation ppp # Đặt encapsulation là PPP
Kiểm tra Ping: Sau khi cấu hình IP và encapsulation, ping giữa R1 và R2 để đảm bảo kết nối vật lý đã hoạt động.

B. Cấu hình PAP trên R1 (Authenticator) và R2 (Peer)
Nguyên lý PAP: Bên yêu cầu (peer) gửi username và password dạng clear-text. Bên xác thực (authenticator) kiểm tra.

Cấu hình trên R1 (Authenticator):

Tạo người dùng local cho R2: Username và password này phải khớp với thông tin mà R2 sẽ gửi.
R1(config)#username R2_peer password cisco # Username R2_peer, password cisco
Áp dụng xác thực PAP trên interface:
R1(config)#interface FastEthernet0/1
R1(config-if)#ppp authentication pap
Cấu hình trên R2 (Peer):

Cấu hình gửi username và password PAP:
R2(config)#interface FastEthernet0/0
R2(config-if)#ppp pap sent-username R2_peer password cisco # Phải khớp với username/password trên R1
C. Kiểm tra PAP
Trên Router R1:

R1#show interface FastEthernet0/1
Tìm dòng "Line protocol is up", "PPP is up" và "LCP is up" và "authentication successful".

R1#show ppp all
Sẽ hiển thị chi tiết trạng thái PPP, bao gồm PAP.

R1#debug ppp authentication
(Để xem quá trình xác thực PAP diễn ra, sau đó tắt bằng no debug ppp authentication hoặc undebug all).

Trên Router R2:

R2#show interface FastEthernet0/0
Tương tự, kiểm tra trạng thái "Line protocol is up" và "PPP is up".

5. Cấu hình CHAP (Challenge-Handshake Authentication Protocol)
Mục tiêu: Cấu hình xác thực CHAP giữa hai Router (bảo mật hơn PAP).

A. Chuẩn bị (Sử dụng lại sơ đồ PAP)
Sử dụng lại sơ đồ mạng và cấu hình IP từ phần PAP.
Gỡ bỏ cấu hình PAP cũ trên R1 và R2: Trên R1:
R1(config)#interface FastEthernet0/1
R1(config-if)#no ppp authentication pap
Trên R2:
R2(config)#interface FastEthernet0/0
R2(config-if)#no ppp pap sent-username R2_peer password cisco
B. Cấu hình CHAP trên R1 (Authenticator) và R2 (Peer)
Nguyên lý CHAP: Bên xác thực gửi một "thử thách" (challenge). Bên yêu cầu tính toán một giá trị băm (hash) dựa trên thử thách và mật khẩu bí mật, sau đó gửi "đáp ứng" (response) lại. Bên xác thực tự tính toán giá trị băm và so sánh. Mật khẩu không bao giờ được gửi qua mạng.

Cấu hình trên R1 (Authenticator):

Tạo người dùng local cho R2: Username phải khớp, mật khẩu phải khớp.
R1(config)#username R2_peer password cisco # Password phải khớp với R2
Áp dụng xác thực CHAP trên interface:
R1(config)#interface FastEthernet0/1
R1(config-if)#ppp authentication chap
Cấu hình trên R2 (Peer):

Cấu hình gửi username cho CHAP:
R2(config)#interface FastEthernet0/0
R2(config-if)#ppp chap hostname R2_peer # Gửi hostname làm username trong CHAP
Lưu ý: CHAP sử dụng hostname của peer làm username. Đảm bảo hostname trên R2 là R2_peer hoặc sử dụng lệnh ppp chap hostname để chỉ định username.
R2(config)#username R1_auth password cisco # Tạo username/password cho bên xác thực (R1_auth)
Lưu ý: Mật khẩu này (cisco) phải khớp với mật khẩu của username R2_peer trên R1. Điều này hơi phức tạp: mỗi bên cần có một mục nhập người dùng cục bộ cho bên kia, và mật khẩu của mục nhập đó phải khớp.
Tóm tắt mật khẩu cho CHAP:

Trên R1, có username R2_peer password cisco.
Trên R2, có username R1_auth password cisco.
Mật khẩu "cisco" phải giống nhau ở cả hai phía để CHAP hoạt động.
C. Kiểm tra CHAP
Trên Router R1:

R1#show interface FastEthernet0/1
Kiểm tra trạng thái "Line protocol is up", "PPP is up" và "LCP is up" và "authentication successful".

R1#show ppp all
Sẽ hiển thị chi tiết trạng thái PPP, bao gồm CHAP.

R1#debug ppp authentication
(Để xem quá trình xác thực CHAP diễn ra, sau đó tắt bằng no debug ppp authentication hoặc undebug all).

Trên Router R2:

R2#show interface FastEthernet0/0
Tương tự, kiểm tra trạng thái "Line protocol is up" và "PPP is up".

IV. Kết luận và Lưu ý
Lưu cấu hình: Sau mỗi phần thực hành, hãy nhớ lưu cấu hình trên các Router bằng lệnh copy running-config startup-config để không bị mất khi khởi động lại.
Debug: Lệnh debug trong IOS rất hữu ích để gỡ lỗi các vấn đề về kết nối và xác thực. Luôn nhớ tắt debug bằng undebug all sau khi hoàn thành để tránh làm đầy console hoặc CPU quá mức.
Bảo mật: PAP không an toàn và chỉ nên dùng trong môi trường lab. CHAP bảo mật hơn nhưng vẫn có những hạn chế nhất định so với các phương pháp xác thực hiện đại hơn như EAP/RADIUS.
Kiến thức cơ bản: Đảm bảo bạn hiểu rõ về địa chỉ IP, subnetting, và cách thiết bị mạng giao tiếp cơ bản trước khi đi sâu vào các giao thức này.
Chúc bạn thực hành thành công và nắm vững kiến thức! Nếu có bất kỳ bước nào gặp khó khăn, đừng ngần ngại hỏi nhé.


Sources





Ubuntu Server kết nối với GNS3 VM như thế nào


Ubuntu Server kết nối với GNS3 VM thông qua một cầu nối mạng được thiết lập trong VMware Workstation (hoặc VirtualBox, tùy thuộc vào phần mềm ảo hóa bạn sử dụng) và được đại diện bằng một Cloud Node trong GNS3. Về cơ bản, bạn đang tạo một "cổng" để máy ảo Ubuntu Server của bạn có thể giao tiếp với mạng ảo bên trong GNS3 VM.

Dưới đây là các bước chi tiết để thiết lập kết nối này:

I. Hiểu Cơ Chế Kết Nối
Khi bạn chạy GNS3 VM trong VMware, VMware sẽ tạo ra các card mạng ảo (virtual network adapters) để GNS3 VM có thể giao tiếp với máy chủ (host machine) và các máy ảo khác. Phổ biến nhất là:

VMnet8 (NAT): Đây là chế độ mặc định và dễ sử cấu hình nhất. VMware tạo một mạng NAT nội bộ, GNS3 VM và các máy ảo khác trong mạng này (như Ubuntu Server của bạn) sẽ nhận địa chỉ IP từ DHCP server của VMware và có thể truy cập Internet thông qua máy chủ của bạn.
VMnet1 (Host-Only): Tạo một mạng riêng giữa máy chủ và các máy ảo, không có truy cập Internet.
VMnet0 (Bridged): Máy ảo sẽ được kết nối trực tiếp vào mạng vật lý của bạn, nhận địa chỉ IP từ router vật lý giống như một thiết bị vật lý khác.
Để Ubuntu Server và GNS3 VM giao tiếp, chúng cần nằm trong cùng một mạng ảo mà VMware quản lý. Chế độ NAT (VMnet8) thường là lựa chọn tốt nhất cho các lab của GNS3 vì nó đơn giản và cung cấp cả kết nối nội bộ lẫn Internet.

II. Các Bước Kết Nối Ubuntu Server với GNS3 VM
Giả sử bạn đã cài đặt GNS3, GNS3 VM trong VMware, và Ubuntu Server cũng trong VMware.

Bước 1: Đảm bảo GNS3 VM và Ubuntu Server đang cùng dải mạng trong VMware
Kiểm tra Network Adapter của GNS3 VM:

Trong VMware Workstation, chọn GNS3 VM.
Nhấp vào Edit virtual machine settings.
Đảm bảo Network Adapter của GNS3 VM đang ở chế độ NAT (VMnet8). Ghi nhớ dải mạng NAT của VMware (ví dụ: 192.168.x.0/24, thường là 192.168.23.0/24 hoặc 192.168.100.0/24).
Cấu hình Network Adapter của Ubuntu Server:

Trong VMware Workstation, chọn Ubuntu Server VM.
Nhấp vào Edit virtual machine settings.
Đảm bảo Network Adapter của Ubuntu Server cũng đang ở chế độ NAT (VMnet8).
Khởi động Ubuntu Server và cấu hình địa chỉ IP cho nó nằm cùng dải với GNS3 VM và dải NAT của VMware.
Bạn có thể dùng DHCP để Ubuntu tự nhận IP: sudo dhclient -r && sudo dhclient (hoặc khởi động lại interface).
Hoặc cấu hình IP tĩnh (khuyến nghị cho lab để dễ quản lý):
Mở file cấu hình mạng (ví dụ: /etc/netplan/00-installer-config.yaml trên Ubuntu Server 18.04+).
Ví dụ cấu hình IP tĩnh (thay ens33 bằng tên interface của bạn, và điều chỉnh dải IP cho phù hợp với dải NAT của VMware):
YAML

network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [192.168.100.2/24] # IP cho Ubuntu Server
      routes:
        - to: default
          via: 192.168.100.1 # Gateway của VMware NAT (thường là .1 hoặc .2)
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2
Áp dụng cấu hình: sudo netplan apply.
Kiểm tra kết nối giữa Ubuntu Server và GNS3 VM (từ Ubuntu): ping <IP_GNS3_VM> (ví dụ: ping 192.168.100.128). IP của GNS3 VM sẽ hiển thị trên màn hình console của nó trong VMware.
Bước 2: Thêm Cloud Node trong GNS3 và kết nối đến VMware Network Adapter
Mở GNS3: Khởi động GNS3 trên máy tính của bạn.
Kéo và Thả Cloud Node:
Trong bảng điều khiển Devices (thường ở bên trái), tìm mục End devices.
Kéo và thả một biểu tượng Cloud vào không gian làm việc của bạn.
Cấu hình Cloud Node:
Click chuột phải vào Cloud vừa kéo vào và chọn Configure.
Trong cửa sổ cấu hình Cloud, chuyển đến tab Ethernet.
Trong phần Generic Ethernet NIO, bạn sẽ thấy danh sách các interface mạng ảo của GNS3 VM mà VMware đã tạo ra.
Chọn interface tương ứng với mạng VMnet8 (NAT) của bạn. Tên của nó thường là vmnet8 (nếu bạn dùng VMware Workstation trên Windows) hoặc một cái gì đó tương tự như Host-only (VMnet8) hoặc Ethernet0 với mô tả về NAT.
Nhấp vào nút Add để thêm interface này vào danh sách "Selected Ethernet adapters".
Nhấp Apply rồi OK.
Bước 3: Nối Router GNS3 với Cloud Node
Kéo và Thả Router IOS: Kéo một Router Cisco IOS (mà bạn đã thêm vào GNS3) vào không gian làm việc.
Nối Dây:
Chọn công cụ Add a link (biểu tượng dây cáp) trên thanh công cụ GNS3.
Click vào Router, chọn một cổng FastEthernet hoặc GigabitEthernet (ví dụ: Fa0/0).
Click vào Cloud node.
GNS3 sẽ hỏi bạn muốn nối với interface nào của Cloud. Hãy chọn interface mà bạn đã cấu hình ở bước trước (ví dụ: Ethernet0 hoặc vmnet8).
Bước 4: Cấu hình Địa chỉ IP trên Router GNS3
Khởi động Router và Console:
Click chuột phải vào Router và chọn Start.
Click chuột phải vào Router lần nữa và chọn Console để mở cửa sổ CLI.
Cấu hình IP trên Router:
Gán địa chỉ IP cho cổng Router được nối với Cloud sao cho nó cùng dải mạng với Ubuntu Server và dải NAT của VMware.
Ví dụ: Nếu Ubuntu Server của bạn là 192.168.100.2 và dải mạng VMware NAT là 192.168.100.0/24, bạn có thể cấu hình cho Router:
Router>enable
Router#configure terminal
Router(config)#interface FastEthernet0/0
Router(config-if)#ip address 192.168.100.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit

