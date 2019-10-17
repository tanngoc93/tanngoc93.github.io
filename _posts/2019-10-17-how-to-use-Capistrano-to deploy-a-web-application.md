---
title:             "Triển khai ứng dụng của bạn với Capistrano"
menutitle:         "Triển khai ứng dụng của bạn với Capistrano"
date:              2019-10-17 00:00:00 +0700
category:          Dev Stories
author:            ngocnt
layout:            post
tags:              
---
Trong quá trình làm việc với NextJS và Wordpress REST API mình đã gặp phải thông báo lỗi khi gửi yêu cầu từ phía NextJS:

```html
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at https://some-url-here. (Reason: additional information here).
```

#### Cross-origin resource sharing là gì?

```html
Cross-origin resource sharing (CORS) là một cơ chế cho phép các tài nguyên bị hạn chế trên một trang web được yêu cầu từ một tên miền khác bên ngoài tên miền mà tài nguyên đầu tiên được phục vụ. Một trang web có thể tự do nhúng các hình ảnh, bảng định kiểu, tập lệnh, iframe và video có nguồn gốc chéo. Các yêu cầu "cross-domain" nhất định, đặc biệt là các yêu cầu Ajax, bị cấm theo mặc định bởi chính sách bảo mật cùng nguồn gốc.
```

Qua giải thích trên chúng ta có thể hiểu được phần nào về CORS, vậy nếu bạn muốn lấy dữ liệu từ Wordpress API REST thì phải làm sao?
Mình tìm trên mạng có khá nhiều hướng dẫn, trong bài viết này mình sẽ kể lại cách mình bật CORS cho máy chủ Wordpress của mình, và nó đang hoạt động tốt.

Step 1: Truy cập vào máy chủ, sau đó truy cập vào thư mục bạn đã cài đặt WordPress codebase, tiếp theo là truy cập vào thư mục ```wp-content``` và khởi tạo thư mục ```mu-plugins```. Bạn có thể hình dung qua đường dẫn bên dưới: <br /> 

```html
/var/www/[wordpress]/wp-content/mu-plugins/
```

Step 2: Bên trong thư mục ```mu-plugins``` tạo một tệp PHP đặt tên tùy ý, mình đặt tên nó là: ```wp-rest-allow-all-cors.php```

Step 3: Sao chép và dán đoạn mã bên dưới vào tệp bạn vừa tạo

```php
<?php
/**
 * Plugin Name: WP-REST-Allow-All-CORS
 * Plugin URI: http://AhmadAwais.com/
 * Description: Allow all cross origin requests to your WordPress site's REST API.
 * Author: mrahmadawais, WPTie
 * Author URI: http://AhmadAwais.com/
 * Version: 1.0.0
 * License: GPL2+
 * License URI: http://www.gnu.org/licenses/gpl-2.0.txt
 *
 * @package WPRAC
**/

// Exit if accessed directly.
if ( ! defined( 'ABSPATH' ) ) {
  exit;
}

// Hook.
add_action( 'send_headers', function() {
  if ( ! did_action('rest_api_init') && $_SERVER['REQUEST_METHOD'] == 'HEAD' ) {
    header( 'Access-Control-Allow-Origin: *' );
    header( 'Access-Control-Expose-Headers: Link' );
    header( 'Access-Control-Allow-Methods: HEAD' );
  }
});

// Hook.
add_action( 'rest_api_init', 'wp_rest_allow_all_cors', 15 );

/**
 * Allow all CORS.
 *
 * @since 1.0.0
**/

function wp_rest_allow_all_cors() {
  // Remove the default filter.
  remove_filter( 'rest_pre_serve_request', 'rest_send_cors_headers' );
  // Add a Custom filter.
  add_filter( 'rest_pre_serve_request', function( $value ) {
    header( 'Access-Control-Allow-Origin: *' );
    header( 'Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, DELETE' );
    header( 'Access-Control-Allow-Credentials: true' );
    return $value;
  });
} // End fucntion wp_rest_allow_all_cors().

```

Step 4: Đăng nhập vào Wordpress Admin Dashboard, truy vập vào phần ```Installed Plugins```, tìm xem phần ```Must-Use``` đã xuất hiện chưa, nếu đã xuất hiện bạn có thể truy cập vào phần ``` Must-Use``` này để kiểm tra xem có tồn tại plugin tên là ```WP-REST-Allow-All-CORS```.

Bài viết tham khảo từ: [AhmadAwais.com](https://github.com/ahmadawais/WP-REST-Allow-All-CORS).<br />
Tham khảo Wordpress Must Use Plugins: [Wordpress.org](https://wordpress.org/support/article/must-use-plugins/).
