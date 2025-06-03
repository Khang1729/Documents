# Github.com
## Sign up
- Đăng ký github: https://github.com/
- Tạo mới Token để lưu trữ Source Code từ máy tính lên Github: https://github.com/settings/tokens/new
- Lưu trữ Token để sử dụng mổi lần muốn lưu Source Code lên Github
## Install Git
- Download và cài đặt Git trên máy tính: https://git-scm.com/downloads



## Install Git
- Step 1: Tải và cài đặt Git: https://git-scm.com/downloads
- Step 2: Đăng ký tài khoản trên github.com
- Step 3: Gửi địa chỉ mail đăng ký github.com cho giảng viên để được mời làm cộng tác viên trên kho SysAdmin
- Step 4: Truy cập kho SysAdmin để theo dõi nộp bài thực hành: https://github.com/dzokha1010/SysAdmin
## Clone and Push to SysAdmin
- Step 1: Mở Command Prompt hoặc CMD trên Windows
- Step 2: Thực hiện câu lệnh clone SysAdmin
  ```
  git clone https://github.com/dzokha1010/SysAdmin.git
  ```
- Step 2: Di chuyển vào thư mục SysAdmin
  ```
  cd SysAdmin
  ```
- Step 3: Tạo tập tin Word đặt tên là MSSV lưu trong thư mục SysAdmin, tập tin này sẻ lưu trữ kết quả mỗi buổi thực hành.
- Step 4: Chụp hình kết quả thực hành từng bài tập và dán vào tập tin Word
- Step 5: Nộp tập tin Word sau buổi thực hành lên kho SysAdmin bằng các câu lệnh
  ```
  git add .  
  git commit -m "<ghi chú>"  
  git push -u origin <name_branch>
  ```
- Lưu ý: Nên tạo Token thay thế mật khẩu để thuận tiện mỗi khi thực hiện câu lệnh Push
  - Link để tạo Token: https://github.com/settings/tokens



## Clone Repository
- Download dữ liệu trên Github: `git clone https://github.com/dzokha1010/HTTT2211.git`
- Trong thư mục hiện tại đã có thư mục tải về, di chuyển vào thư mục tải về: `cd HTTT2211`
- Hiển thị các file trong thư mục: `ls`
- Hiển thị đường dẫn đến thư mục: `pwd`
## Modify
- Lưu file thay đổi: `git add .`
- Ghi lại trạng thái thay đổi: `git commit -m "<ghi chú>"`
- Đẩy những thay đổi từ máy tính lên Github: `push -u origin <name_branch>`
- Đồng bộ từ Github về máy tính: `git pull`
## Managing remote repositories
Để đẩy Folder từ máy tính lên Github mà không thực hiện clone, chúng ta thực hiện câu lệnh sau trong thư mục
- Liên kết thư mục với repo muốn đưa Source Code lên: `git remote add origin https://github.com/dzokha1010/HTTT2211.git`
- Liệt kê các liên kết đang sử dung: `git remote -v`
- Đảy dữ liệu lên Github: `git push origin master`
## Config
- Liệt kê thiết lập: `git config -l`
- Thiết lập tài khoản Github: `git config user.name <user_github>`
- Thiết lập email đã đăng ký trên Github: `git config user.email <mail_user_github>`
- Lựa chọn trình soạn thảo mặc định: `git config --global core.editor vi`
## Branch
- git branch -M <name_branch>
- git checkout <name_branch>
## Error 400
- git config http.postBuffer 52428800
