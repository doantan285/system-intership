# Cây thư mục Nginx

## CentOS 7 (httpd)

| Đường dẫn | Chức năng |
|-----------|-------------|
| `/etc/nginx/nginx.conf` | File cấu hình chính của Nginx, chứa các thiết lập chung như worker_processes, logging, gzip, v.v. |
| `/etc/nginx/conf.d/` | Thư mục chứa các file cấu hình VirtualHost (server block). |
| `/etc/nginx/conf.d/default.conf` | File VirtualHost mặc định của Nginx, có thể sửa hoặc tạo file mới trong thư mục `conf.d/`. |
| `/usr/share/nginx/html/` | Thư mục gốc chứa file website mặc định (tương tự /var/www/html/ trên Apache). |
| `/var/log/nginx/access.log` | File log ghi lại mọi request truy cập vào server. |
| `/var/log/nginx/error.log` | 	File log ghi lại lỗi của Nginx. |
| `/usr/lib/systemd/system/nginx.service` | File quản lý dịch vụ Nginx khi dùng `systemctl`. |
| `/etc/nginx/modules/` | Thư mục chứa các module Nginx. |

**Lưu ý:**

- Nếu muốn chạy nhiều website trên Nginx, nên tạo file cấu hình riêng trong `/etc/nginx/conf.d/` thay vì sửa trực tiếp nginx.conf.
- Để bật gzip hoặc cấu hình proxy, sửa file `nginx.conf`.

## Trên Ubuntu (Apache2)

| Cột 1 | Cột 2 |
|-----------|-------------|
| `/etc/nginx/nginx.conf` | File cấu hình chính của Nginx, tương tự CentOS. |
| `/etc/nginx/sites-available/` | Thư mục chứa các file VirtualHost (cần kích hoạt bằng `ln -s`). |
| `/etc/nginx/sites-enabled/` | Thư mục chứa các VirtualHost đã được kích hoạt. |
| `/etc/nginx/modules-enabled/` | Chứa các module đã được kích hoạt. |
| `/usr/share/nginx/html/` | Thư mục gốc chứa file website mặc định. |
| `/var/log/nginx/access.log` | File log ghi lại mọi request truy cập vào server. |
| `/var/log/nginx/error.log` | File log ghi lại lỗi của Nginx. |
| `/usr/lib/systemd/system/nginx.service` | File quản lý dịch vụ Nginx khi dùng `systemctl`. |

**Lưu ý:**

- Trên Ubuntu, cấu hình VirtualHost nằm trong `/etc/nginx/sites-available/`, cần kích hoạt bằng lệnh:

    ```plaintext
    sudo ln -s /etc/nginx/sites-available/ten_file.conf /etc/nginx/sites-enabled/
    sudo systemctl restart nginx
    ```

- Để vô hiệu hóa VirtualHost, xóa liên kết trong `sites-enabled/`:

    ```plaintext
    sudo rm /etc/nginx/sites-enabled/ten_file.conf
    sudo systemctl restart nginx
    ```
