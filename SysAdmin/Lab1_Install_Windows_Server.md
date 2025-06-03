# Laboratory 1 - Installing Windows Server 2022
## Introduction
- Windows Server 2022 Datacenter: Dành cho tổ chức cần môi trường ảo hóa cao và đám mây riêng. Hỗ trợ đầy đủ tính năng của Windows Server với số lượng máy ảo không giới hạn.
- Windows Server 2022 Standard: Phù hợp với tổ chức có hệ thống vật lý hoặc ít máy ảo. Cung cấp đầy đủ tính năng Windows Server nhưng chỉ hỗ trợ tối đa 2 máy ảo.
- Windows Server 2022 Azure Edition: Thiết kế riêng để chạy trên nền tảng đám mây Azure, hoạt động như một máy ảo trên Azure IaaS hoặc cụm Azure Stack HCI.
- Windows Server 2022 Essentials: Lý tưởng cho doanh nghiệp nhỏ (tối đa 25 người dùng, 50 thiết bị). Giao diện đơn giản hơn, tích hợp sẵn kết nối với dịch vụ đám mây nhưng không hỗ trợ ảo hóa.
## Download Windows Server 2022
- Link Download: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
## Download VMware Workstation
- Link Dowload: https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
## Create Virtual Machine
- Step 1: Khởi động VMware Workstation Pro
- Step 2: File/Mew Virtual Machine
- Step 3: Chọn Typical, Next
- Step 4: Chọn I will install the operating system later, Next
- Step 5: Chọn Microsoft Windows, chọn Version: Windows Server 2022, Next
- Step 6: Đặt tên Virtual machine, nên đặt tên dễ nhớ để thực hành các buổi sau, chọn đường dẫn lưu trữ đến ổ D, Next
- Step 7: Chọn Maximum disk size 60GB, chọn Split virtual disk into multipe files, Next
- Step 8: Finish
- Step 9: Click chuột phải vào Virtual Machine vừa tạo bên cửa sổ tay trái, Chọn Settings
- Step 10: Click vào CD/DVD (SATA), chọn Use ISO image file và click Brower… để dẫn đến tập tin Windows Server 2022.ISO, Ok
## Installing Windows Server 2022
Windows Server 2022 có 03 chế độ cài đặt:
  - Windows Server 2022 (Desktop Experience)
  - Windows Server 2022 Server Core
  - Windows Server 2022 Nano Server

Các bước cài Windows Server 2022 (Desktop Experience):
- Step 1: Click chuột phải vào Virtual Machine vừa tạo và bấm Powrer/Power On
- Step 2: Click chuột vào màn hình màu đen và bấm phím bất kỳ để vào giao diện Windows Setup
- Step 3: Tại giao diện Windows Setup, Next
- Step 4: Chọn Install now
- Step 5: Tại giao diện Windows Setup hiển thì 4 tuỳ chọn, chọn Windows Server 2022 Datacenter Evaluation (Desktop Experience), Next
- Step 6: Chọn I accept the Microsoft…, Next
- Step 7: Màn hình Which type of installation do you want?, chọn Custom: Install Micorsoft Server ….
- Step 8: Chọn New để tạo ổ đỉa (phân vùng) mới, Chọn Apply, Chọn Ok, Chọn Next
- Step 9: Đặt password cho tài khoản Administrator
- Step 10: Trên thanh công cụ của VMware thực hiện chọn Tab VM/Send Ctrl + Alt + Del
