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
1. Cấu hình Syslog Server (Ubuntu Server)
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
- Cấu hình Router trên GNS3
```
conf t
logging <IP_syslog_server>
logging trap informational
logging on
```
- Kiểm tra Log nhận được trên Server
2. Cấu hình SSH
- Trên Router Cisco:
```
conf t
hostname R1
ip domain-name local
crypto key generate rsa
username admin privilege 15 secret admin123
line vty 0 4
login local
transport input ssh
```
- Trên Máy tính Client kiểm tra kết nối SSH: `ssh admin@<IP_router>`
3. Cấu hình NTP
- Cài đặt NTP Server trên Ubuntu Server
```
sudo apt install ntp
sudo nano /etc/ntp.conf
```
- Trên Router Cisco
```
conf t
ntp server <IP_NTP_server>
show ntp associations
```
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

