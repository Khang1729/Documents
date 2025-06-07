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
+ Cấu hình NTP Server:
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
- Cài đặt FreeRADIUS trên Ubuntu Server (RADIUS Server): `sudo apt install freeradius`
- Cấu hình người dùng và phương thức xác thực PAP/CHAP.
- Trên Router cấu hình RADIUS và xác thực PPP:
```
conf t
aaa new-model
aaa authentication ppp default group radius
radius-server host <IP_radius> key radius123
interface serial0/0
encapsulation ppp
ppp authentication chap pap
```
- Kiểm tra kết nối từ client/router khác (mô phỏng PPP).

