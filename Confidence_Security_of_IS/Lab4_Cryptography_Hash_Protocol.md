
# Laboratory 4 - Cryptography, Hash Function Applications and Secure Communication Protocol
## I. Introduction


## II. Practical Application
- Thử nghiệm các phương pháp mật mã đã học bằng công cụ online:

  Link 1: https://legacy.cryptool.org/en/cto/

  Link 2: https://www.cryptool.org/en/cto/

  Link 3: https://cryptoknife.com/

- Cài đặt OpenSSL thiết lập giao thức SSL cho Website

  Link: https://www.openssl.org/
  
  Phiên bản cài đặt cho Windows: https://slproweb.com/products/Win32OpenSSL.html

- Cấu hình SSL cho localhost

  B1: Create Private Key
  ```
  openssl genrsa -des3 -out rootSSL.key 2048
  ```
  B2: Create Certificate File
  ```
  openssl req -x509 -new -nodes -key rootSSL.key -sha256 -days 1024 -out rootSSL.pem
  ```
  B3: Create a local domain.
  Thêm dòng sau vào file hosts trong thư mục c:\program files\windows\system32\drivers\etc\
  ```
  localhost 127.0.0.1 domain-new.local
  ```
  B4: Create a Private Key for the New Domain.
  Thêm dòng sau vào file hosts trong thư mục c:\program files\windows\system32\drivers\etc\
  ```
  openssl req -new -sha256 -nodes -out domain-new.local.csr -newkey rsa:2048 -keyout domain-new.local.key -subj "/C=US/ST=GA/L=Tifton/O=Client One/OU=Dev/CN=domain-new.local/emailAddress=hello@domain-new.local"
  ```
  
  
 Sinh viên có mặt: 2311055, 03, 04, 05, 07, 09, 14, 15L, 16, 18, 21, KMT221124, 25, 26, 31, 32, 35, 36, 42, 43, 44, 48,53,55, 57, 60,28,
 
 Sinh viên có mặt: 01,006,13,17,19,20,22,23,27,29,34,37,38,40,45,49,52,54,58,59,61,62,63

 sinh viên xin vắng: 30
  
