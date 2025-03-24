# Cây thư mục Apache

## CentOS 7 (httpd)

| Đường dẫn | Chức năng |
|-----------|-------------|
| `/etc/httpd/conf/httpd.conf` | File cấu hình chính của Apache trên CentOS. Mọi thay đổi lớn về VirtualHost, cổng lắng nghe, logging đều thực hiện ở đây. |
| `/etc/httpd/conf.d/` | Thư mục chứa các file cấu hình bổ sung (VirtualHost, SSL, PHP, v.v.). |
| `/etc/httpd/conf.d/vhost.conf` | File chứa cấu hình VirtualHost (nếu không có, cần tạo thủ công). |
| `/etc/httpd/modules/` | Thư mục chứa các module của Apache. |
| `/var/www/html/` | Thư mục gốc chứa file website mặc định. |
| `/var/log/httpd/access_log` | File log ghi lại mọi request truy cập vào server. |
| `/var/log/httpd/error_log` | File log ghi lại lỗi của Apache. |
| `/usr/lib/systemd/system/httpd.service` | File quản lý dịch vụ Apache khi dùng `systemctl`. |

**Lưu ý:**

- Nếu muốn cấu hình VirtualHost, cần sửa `/etc/httpd/conf.d/vhost.conf` hoặc thêm file `.conf` mới trong thư mục /etc/httpd/conf.d/.
- Muốn bật mod_rewrite hoặc các module khác, kiểm tra trong `/etc/httpd/conf.modules.d/`.

## Trên Ubuntu (Apache2)

| Cột 1 | Cột 2 |
|-----------|-------------|
| `/etc/apache2/apache2.conf` | File cấu hình chính của Apache trên Ubuntu. |
| `/etc/apache2/ports.conf` | Cấu hình các cổng lắng nghe của Apache. |
| `/etc/apache2/sites-available/` | Thư mục chứa cấu hình VirtualHost (cần kích hoạt bằng `a2ensite`). |
| `/etc/apache2/sites-enabled/` | Thư mục chứa các VirtualHost đã kích hoạt. |
| `/etc/apache2/mods-available/` | Chứa danh sách các module có thể bật/tắt. |
| `/etc/apache2/mods-enabled/` | Chứa các module đã được bật. |
| `/var/www/html/` | Thư mục gốc chứa file website mặc định. |
| `/var/log/apache2/access.log` | File log ghi lại mọi request truy cập vào server. |
| `/var/log/apache2/error.log` | File log ghi lại lỗi của Apache. |
| `/usr/lib/systemd/system/apache2.service` | File quản lý dịch vụ Apache khi dùng `systemctl`. |

**Lưu ý:**

- Để bật VirtualHost:

    ```plaintext
    sudo a2ensite ten_file.conf
    sudo systemctl restart apache2
    ```

- Để tắt VirtualHost:

    ```plaintext
    sudo a2dissite ten_file.conf
    sudo systemctl restart apache2
    ```

- Để bật mod_rewrite:

    ```plaintext
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    ```
