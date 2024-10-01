# Documents
## Install Git
Download và Install Git: https://git-scm.com/downloads
## Clone Repository
- git clone https://github.com/dzokha1010/HTTT2211.git
- cd HTTT2211
## Managing remote repositories
- git remote add origin https://github.com/dzokha1010/HTTT2211.git
- git remote -v
## Config
- git config -l
- git config user.name <user_github>
- git config user.email <mail_user_github>
## Branch
- git branch -M <name_branch>
- git checkout <name_branch>
## Modify
- git add .
- git commit -m "<ghi chú>"
- git push -u origin <name_branch>
## Error 400
- git config http.postBuffer 52428800
