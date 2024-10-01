# Markdown
Tham khảo: http://daringfireball.net/projects/markdown/syntax
## Mục lục
[1. Thẻ tiêu đề](#thetieude)

[2. Chèn link, hình](#chenlinkhinh)

[3. Ký tự in đậm, in nghiêng](#kytuindaminnghieng)

[4. Trích dẫn, bo chữ](#trichdanbochu)

[5. Gạch đầu dòng](#gachdaudong)

[6. Tạo bảng](#taobang)



<a name="thetieude"></a>
### 1. Thẻ tiêu đề

Markdown sử dụng kí tự # để bắt đầu cho các thẻ tiêu đề, có thể dùng từ 1 đến 6 ký tự # liên tiếp.

Ví dụ:

```
# 1.Tiêu đề cấp 1
```

<a name="chenlinkhinh"></a>
### 2. Chèn link, hình

Để chèn hyperlink bạn chỉ cần paste luôn linh đó vào file .md

```
https://github.com
```

Hoặc bạn cũng có thể sử dụng cú pháp sau để thu ngắn đường dẫn của link

```
[Github](https://github.com)
```

Để chèn ảnh thì bạn hãy sử dụng cú pháp sau:

```
<img src="link_anh_cua_ban">
```

Bạn có thể up hình đó lên trang http://i.imgur.com/ để lấy đường dẫn ảnh đưa vào Github

<a name=kytuindaminnghieng></a>
### 3. Ký tự in đậm, in nghiêng

- Để in đậm một đoạn text  bạn chỉ cần làm như sau:

```
**từ cần in đậm**
```

- Để in nghiên một đoạn text  bạn chỉ cần làm như sau:

```
*từ cần in nghiêng*
```

<a name="trichdanbochu"></a>
### 4. Trích dẫn, bo chữ

Để bo một đoạn text thì bạn chỉ cần sử dụng cú pháp sau:

```
`đoạn cần bo`
```

Để làm nổi bật một đoạn, chẳng hạn như một đoạn shell hay file cấu hình bạn có thể sử dụng cú pháp như ví dụ sau:

    ```sh
    auto eth0
    iface eth0 inet static
    ipaddress 10.10.10.10
	netmask 255.255.255.0
	gateway 10.10.10.1
	dns-nameservers 8.8.8.8
    ```

<a name="gachdaudong"></a>
### 5. Gạch đầu dòng

Để sử dụng gạch đầu dòng bạn chỉ cần sử dụng cú pháp sau:

```
- Gạch đầu dòng thứ nhất
  
  - Thụt với đầu dòng 1
  
  - Thụt với đầu dòng 1
 
- Gạch đầu dòng thứ hai
  
  - Thụt với đầu dòng 2
  
  - Thụt với đầu dòng 2
  
```

<a name="taobang"></a>
### 6. Tạo bảng

Bạn có thể sử dụng cú pháp sau để tạo bảng:

```
| Cột 1 Hàng 1 | Cột 2 | Cột 3| Cột 4 |
|--------------|-------|------|-------|
| Hàng 2 | 2 x 1 | 2 x 2 | 2 x 3 | 2 x 4 |
| Hàng 3 | 3 x 1 | 3 x 2 | 3 x 3 | 3 x 4 |
| Hàng 4 | 4 x 1 | 4 x 2 | 4 x 3 | 4 x 4 |
```
