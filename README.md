# Documents
## Github
- Đăng ký github: https://github.com/
- Tạo mới Token để lưu trữ Source Code từ máy tính lên Github: https://github.com/settings/tokens/new
- Lưu trữ Token để sử dụng mổi lần muốn lưu Source Code lên Github
## Install Git
- Download và cài đặt Git trên máy tính: https://git-scm.com/downloads
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
