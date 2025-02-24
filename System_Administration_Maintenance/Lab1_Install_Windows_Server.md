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
B7: Chọn Maximum disk size 60GB, Chọn Split virtual disk into multipe files, Next
B8: Finish
B9: Click chuột phải vào tên Virtual Machine vừa đặt bên cửa sổ tay trái, Chọn Settings …
B10: Click vào CD/DVD (SATA), chọn Use ISO image file và click Brower… để dẫn đến file Windows Server 2022.ISO vừa tải về, Nhấp vào Ok

- Có 3 lựa chọn khi cài đặt: [tài liệu hướng dẫn](https://docs.google.com/document/d/1rB1SqUjgfCIueU7rSO3_WvkVXM85tsOabo93QS-Dh7I/edit?usp=sharing)
1. Windows Server 2022 (Desktop Experience)
2. Windows Server 2022 Server Core
3. Windows Server 2022 Nano Server
