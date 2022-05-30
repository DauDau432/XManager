# Bảng điều khiển XManager
## 0 Dùng thử miễn phí
Bản dùng thử miễn phí giới hạn số lượng người dùng là 20 người và tất cả các chức năng đều có sẵn.

## 1 Cài đặt XManager
Triển khai thủ công bằng aaPanel

aaPanel là phiên bản quốc tế của Pagoda (bt.cn)

Định cấu hình aaPanel
 
Bạn cần chọn hệ thống của mình trong aaPanel để lấy phương pháp cài đặt. Ở đây, CentOS 7+ được sử dụng làm môi trường hệ thống để cài đặt

Hãy chắc chắn sử dụng Centos 7+ để cài đặt Aapanel và các hệ thống khác có thể có vấn đề không xác định.
Kịch bản mới nhất có thể được lấy trên trang web chính thức của Aapanel

```
yum install -y wget && wget -O install.sh http://www.aapanel.com/script/install_6.0_en.sh && bash install.sh
```

Sau khi cài đặt hoàn tất, chúng tôi đăng nhập vào Aapanel để cài đặt môi trường.

Chọn phương pháp cài đặt môi trường bằng LNMP và kiểm tra thông tin sau

☑️ Nginx / Apache

☑️ MySQL

☑️ PHP 7.4

Chọn Fast để nhanh chóng biên dịch và cài đặt.

Cài đặt Ioncube, fileinfo

☑️ aaPanel> App Store> Tìm PHP 7.4 và nhấp vào Cài đặt> Cài đặt vùng mở rộng> `Ioncube` `fileinfo` để cài đặt.

Bỏ chặn các chức năng bị cấm

☑️ aaPanel> App Store> Tìm PHP 7.4 Nhấp vào Setting> Disabled functions để xóa hệ thống `putenv` `proc_open` khỏi danh sách.

Thêm trang web

☑️ aaPanel> Trang web> Thêm trang web.

Điền vào tên miền bạn trỏ đến máy chủ trong Miền

Chọn MySQL trong Cơ sở dữ liệu

Chọn PHP-7.4 tại PHP Verison

Khởi động lại PHP

☑️ Sau khi đăng nhập vào máy chủ qua SSH, truy cập vào đường dẫn trang web như: /www/wwwroot/tên miền trang web của bạn.

Tất cả các lệnh sau đây đều cần được thực thi trong thư mục trang web.

xóa các tập tin trong thư mục

``` 
chattr -i .user.ini
```
```
rm -rf .htaccess 404.html index.html .user.ini
```
Thực thi lệnh sao chép từ Github vào thư mục hiện tại.
```
git clone https://github.com/xcode75/XManager.git tmp -b main && mv tmp/.git . && rm -rf tmp && git reset --hard
```
Cài đặt composer
```
php composer.phar update -n
```
```
cd /www/wwwroot/tên miền trang web của bạn/storage/geodata
```
```
unzip ip2locips.zip
```
Sao chép tệp `/www/wwwroot/tên miền trang web của bạn/smarty_internal_resource_file.php` trong thư mục trang web vào thư mục `/www/wwwroot/tên miền trang web của bạn/vendor/smarty/smarty/libs/sysplugins`
```
cp /www/wwwroot/tên miền trang web của bạn/smarty_internal_resource_file.php  /www/wwwroot/tên miền trang web của bạn/vendor/smarty/smarty/libs/sysplugins/
```
Đặt quyền thư mục trang web thành 777
```
chmod -R 777 /www/wwwroot/tên miền trang web của bạn
```
Chỉnh sửa cấu hình `/www/wwwroot/tên miền trang web của bạn/config/config.php,` điền thông tin cơ sở dữ liệu (muốn mở bản sao lưu XManager, cơ sở dữ liệu phải sử dụng root và mật khẩu gốc).

Tạo cơ sở dữ liệu, CHARSET chọn utf8mb4

Sử dụng phpmyadmin để nhập cơ sở dữ liệu sql `/xmanager.sql` không sử dụng aapanal để nhập sql

Định cấu hình thư mục trang web và giả tĩnh

☑️ Chỉnh sửa trang đã thêm sau khi thêm > Site directory > Running directory Chọn `/public` để lưu.

☑️ Sau khi thêm, hãy chỉnh sửa trang web đã thêm> Viết lại URL và điền thông tin giả tĩnh.

Máy chủ web Nginx
-----------------------------------------------
```
location / {
    try_files $uri /index.php$is_args$args;
}
```
☑️ Khởi động lại nginx
```
service nginx restart
```
```
systemctl restart nginx
```
Máy chủ web Apache
-------------------------------------------------

☑️ Apache yêu cầu `.htaccess` `/www/wwwroot/yoursitedomain/.htaccess` và `/www/wwwroot/yoursitedomain/public/.htaccess`

Đổi tên `.htaccess.bak` thành `.htaccess`

Định cấu hình cron job cronjob

☑️ SSH chạy lệnh `crontab -e`

☑️ Nhấn `A` trên bàn phím và thêm  `* * * * * cd /www/wwwroot/your_site_domain && php cron.php 1 >> /dev/null 2>&1`

Sau khi thêm, nhấn `ESC` trên bàn phím và nhập `:x` rồi nhấn ENTER trên bàn phím

NB: Thay đổi đường dẫn thư mục trang web của bạn `/www/wwwroot/tên miền trang web của bạn`

Tạo quản trị viên
```
cd /www/wwwroot/tên miền trang web của bạn
```
```
php bin/console.php createAdmin
```
Tải xuống ứng dụng
```
cd /www/wwwroot/tên miền trang web của bạn
```
```
php bin/console.php downloadapps  
```
Thiết lập Telegram Webhooks
```
cd /www/wwwroot/tên miền trang web của bạn
```
```
php bin/console.php setTelegram
```
Phần phụ trợ quản lý, cài đặt hệ thống -> cài đặt thông báo -> phương thức thông báo -> Thông tin điện tín

Trò chuyện nhóm ID TG Chat ID TG robot bot gửi /ping để nhận ID

TG Group ID: Thêm bot vào nhóm TG và sau đó /ping để lấy ID

Chìa khóa TG BotFather nhận chìa khóa (token)

## 2. Nâng cấp Xmanager
nâng cấp
---------------------------------------------
Sau khi đăng nhập vào máy chủ qua SSH, hãy truy cập vào đường dẫn của trang web như:
```
cd /www/wwwroot/tên miền trang web của bạn
```
lệnh nâng cấp
```
php bin/console.php update
```
