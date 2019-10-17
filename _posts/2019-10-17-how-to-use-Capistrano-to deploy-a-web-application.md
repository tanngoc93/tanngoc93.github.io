---
title:             "Triển khai webApp của bạn với Capistrano"
menutitle:         "Triển khai webApp của bạn với Capistrano"
date:              2019-10-17 00:00:00 +0700
category:          Dev Stories
author:            ngocnt
layout:            post
tags:              
---
### Phân biệt Web server và Application server

Web server là một chương trình nhận request từ user và xử lý một số thứ trên request đó. Sau đó nó đưa request đó cho webApp. Nginx và Apache là hai web server mà chúng ta thường dùng nhất. Một trong những khả năng của web server là xử static files(JS/CSS/Images), những static files là những file tĩnh và không cần phải xử lý thông qua webApp, đều này giúp tăng tốc độ ứng dụng, giảm tải cho Application server.

Application server có nhiệm vụ tải code và giữ trong bộ nhớ. Khi app server nhận một request từ web server nó sẽ báo lại cho bên webApp biết. Sau khi app xử lý xong request app server sẽ gửi lại response cho web server (và thậm chí gửi tới user).

```
* Các Application server phổ biến trong Rails: Mongrel(không được dùng nhiều nữa), Unicorn, Thin, Rainbows và Puma.
```

```
* Phusion Passenger về cơ bản được xây dựng dựa trên Nginx core, nó là một cái hơi duy nhất vì trong standalone mode nó làm việc như một Application server. Nhưng nó cũng có thể build thành một Web server vậy không cần một Application server riêng biệt để chạy webApp nữa. Hiện tại Passenger 6 đã support hầu hết các ngôn ngữ Ruby / Node / Golang / Elixir, etc.
```

### Hình dung quy trình:

#### Setup server first…!
Bất kể ứng dụng của bạn là Ruby/NodeJS/Golang thì bạn phải tạo nguyên một cái server mới, sau đó là quá trình cài đặt cơ bản như Security, SSH, etc. Sau đó nữa là bạn phải install các gói phần mềm để chạy cái webApp của bạn…!

#### Thời cổ đại
Nếu triển khai theo kiểu thủ công giống như thời cổ đại thì chúng ta sẽ tạo một cái server ở đâu đó, sau đó dùng FTP software để connect vào cái server đó và Push code-base trực tiếp từ local lên server, rồi bla bla bla cho tới khi có thể truy cập bằng trình duyệt

#### Sau thời cổ đại là thời hậu cổ đại:
Lúc này đã có Github / Gitlab, etc. Chỉ thế thôi…! Thì công việc của ta lúc này rất là khỏe, viết code xong, sau đó là Push code-base lên Github chẳng hạn. Tiếp nữa là connect vào server trên kia thông qua SSH và Pull code-base từ Git Repository xuống(Yêu cầu duy nhất là mình phải tạo SSH key ở server và lấy cái SSH key này Add vào Github account của mình để server có quyền truy cập vào Git Repository trước đã).

#### Thời hiện đại với Capistrano:
Tân dụng kiến thức xã hội học của thời mông cổ, chúng ta đã có được Github / Gitlab, etc, một anh developer thất tình vì không đủ thời gian chăm sóc bồ nhí đã sáng tạo ra một công cụ tuyệt vời để giúp đơn giản việc triển khai ứng dụng Ruby on Rails, công cụ đó tên là Capistrano. Lúc đầu Capistrano chỉ phục vụ mục đích chính là triển khai ứng dụng xây dựng trên nền tảng Ruby / Rails, những năm sau đó Capistrano đã hướng dần sang mục tiêu trở thành công cụ triển khai cho hầu hết các webApp viết trên bất kỳ ngôn ngữ nào...!
