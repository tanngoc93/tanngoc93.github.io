---
title:             "Cách dùng file robots.txt để tối ưu website của bạn!"
menutitle:         "Cách dùng file robots.txt để tối ưu website của bạn!"
date:              2019-06-06 00:00:00 +0700
category:          Dev Stories
author:            ngocnt
layout:            post
tags:              
---
Vừa rồi mình đã làm qua một nhiệm vụ xử lý tệp `robots.txt` cho `staging` và `production`, mục tiêu là ẩn hết thông tin của `staging` khỏi công cụ tìm kiếm. Tuy cũng hay làm về mảng SEO marketing nhưng kiến thức của mình khá lộn xộn nên mình viết bài này để chia sẻ và lưu lại cho riêng mình sau này!!!

Theo mình biết thì mục tiêu của file `robots.txt` là để nói với các công cụ tìm kiếm(Google search, Bing search,...) những tập tin nào nên và không nên được lập chỉ mục bởi chúng. Thường thì, nó được sử dụng để chỉ định các tệp không được lập chỉ mục bởi các công cụ tìm kiếm.

#### Lập chỉ mục là gì?
Lập chỉ mục là quá trình công cụ tìm kiếm truy cập vào website của bạn và thu thập tất cả các trang sau đó lưu vào cơ sở dữ liệu của chúng, khi người dùng tìm kiếm những nội dung này sẽ xuất hiện trên kết quả tìm kiếm của họ.

Để cho phép các công cụ tìm kiếm thu thập dữ liệu và lập chỉ mục toàn bộ nội dung trang web của bạn, bạn có thể thêm các dòng sau vào tệp robots.txt của mình:

```html
  User-agent: *
  Disallow:
```

Mặt khác, nếu bạn không muốn cho phép trang web của mình bị lập chỉ mục hoàn toàn, bạn có thể sử dụng các dòng dưới đây:

```html
  User-agent: *
  Disallow: /
```

Để có kết quả nâng cao hơn, bạn sẽ cần hiểu các phần trong tệp robots.txt. Dòng "User-agent:" chỉ định các công cụ mà cài đặt sẽ hợp lệ. Bạn có thể sử dụng "*" làm giá trị để tạo quy tắc cho tất cả các bot tìm kiếm hoặc tên của bot bạn muốn tạo quy tắc cụ thể.

Phần "Disallow:" để xác định các tệp và thư mục không được lập chỉ mục bởi các công cụ tìm kiếm. Mỗi thư mục hoặc tệp phải được xác định trên một dòng mới. Ví dụ: các dòng bên dưới sẽ yêu cầu tất cả các công cụ tìm kiếm không lập chỉ mục các thư mục "security" và "security" trong thư mục public_html của bạn:

```html
  User-agent: *
  Disallow: /private
  Disallow: /security
```

Xin lưu ý rằng câu lệnh "Disallow:" sử dụng thư mục gốc trang web của bạn làm thư mục cơ sở, vì vậy đường dẫn đến các tệp của bạn phải là /sample.txt chứ không phải /home/user/public_html/sample.txt.

Bài viết tham khảo từ: [SiteGround.com](https://www.siteground.com/kb/how_to_use_the_robotstxt_file/).
