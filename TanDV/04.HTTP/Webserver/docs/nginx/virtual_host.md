# Virtual Host

## Virtual Host trong Nginx

Trong Nginx, Virtual Host (hay còn gọi là Server Block) là một cấu trúc cho phép chạy nhiều website khác nhau trên cùng một máy chủ (server) duy nhất. Nó tương tự như khái niệm Virtual Host trong Apache.

Hiểu đơn giản: Tưởng tượng một máy chủ vật lý như một tòa nhà văn phòng lớn. Mỗi Virtual Host là một "văn phòng" riêng biệt bên trong tòa nhà đó. Mỗi "văn phòng" có địa chỉ riêng (tên miền) và có thể chứa một website hoàn toàn khác biệt.

**Chức năng chính của Virtual Host trong Nginx:**

- **Cho phép hosting nhiều tên miền trên một địa chỉ IP:**  Có thể chạy nhiều website với các tên miền khác nhau (ví dụ: example.com, another-site.net, my-blog.org) trên cùng một máy chủ có một địa chỉ IP duy nhất.
- **Phân chia cấu hình cho từng website:** Mỗi Virtual Host có một khối cấu hình riêng, cho phép bạn tùy chỉnh các thiết lập riêng biệt cho từng website, chẳng hạn như:
  - Thư mục gốc của website (root directory).
  - Các quy tắc xử lý request (ví dụ: chuyển hướng, rewrite).
  - Chứng chỉ SSL/TLS cho HTTPS.
  - Các thiết lập liên quan đến bảo mật và hiệu suất.
- **Quản lý dễ dàng:** Có thể quản lý từng website một cách độc lập thông qua các file cấu hình Virtual Host riêng biệt.

**Cách Nginx xác định Virtual Host nào sẽ xử lý request:**
Khi một người dùng truy cập vào một website trên server của bạn, Nginx sẽ xem xét các yếu tố sau để xác định Virtual Host nào sẽ xử lý request đó:

- **Tên miền (Server Name):** Đây là yếu tố quan trọng nhất. Nginx sẽ so sánh tên miền trong request của người dùng với các tên miền được cấu hình trong các Virtual Host.
- **Địa chỉ IP và cổng (Listen Directive):** Bạn có thể cấu hình Virtual Host lắng nghe trên một địa chỉ IP và cổng cụ thể.
- **Mặc định (Default Server):** Bạn có thể chỉ định một Virtual Host là mặc định. Virtual Host này sẽ xử lý các request không khớp với bất kỳ Virtual Host nào khác.

## Cấu hình nhiều website trên cùng một webserver

### Trên CentOS 7

`Bước 1`: Tạo thư mục và file cho từng website

Giả sử có 2 website:

- censite1.com
- censite2.com

Tạo thư mục chứa nội dung web:

```plaintext
sudo mkdir -p /var/www/censite1.com/public_html
sudo mkdir -p /var/www/censite2.com/public_html
```

Tạo file `index.html` cho từng site:

```plaintext
echo "<h1>welcome to censite1.com</h1>" | sudo tee /var/www/censite1.com/public_html/index.html

echo "<h1>welcome to censite2.com</h1>" | sudo tee /var/www/censite2.com/public_html/index.html
```

- `echo "..." | sudo tee file.html`: Ghi nội dung vào file.

Cấp quyền cho thư mục web:

```plaintext
sudo chown -R nginx:nginx /var/www/censite1.com/
sudo chown -R nginx:nginx /var/www/censite2.com/
sudo chmod -R 755 /var/www
```

- `chown -R nginx:nginx`: gán quyền sở hữu cho user `nginx`.
- `chmod -R 755`: Cấp quyền đọc và ghi cho thư mục.

`Bước 2`: Cấu hình VirtualHost trong Nginx

Tạo file cấu hình cho từng website:

```plaintext
sudo vi /etc/nginx/conf.d/censite1.com.conf
```

Thêm nội dung sau:

```plaintext
server {
    listen 80;
    server_name censite1.com;
    root /var/www/censite1.com/public_html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Tương tự tạo file cho censite2com:

```plaintext
sudo vi /etc/nginx/conf.d/censite2com.conf
```

Thêm nội dung sau:

```plaintext
server {
    listen 80;
    server_name censite2.com;
    root /var/www/censite2.com/public_html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

- `Listen 80`: Lắng nghe trên cổng 80.
- `server_name`: Tên miền của website.
- `root`: Thư mục chứa website.
- `index`: File mặc định sẽ được tải khi truy cập.
- `location /`: Xử lý yêu cầu đến thư mục gốc.

`Bước 3`: Kiểm tra và khởi động lại Nginx

Kiểm tra cấu hình:

```plaintext
sudo nginx -t
```

Kết quả đúng:

![success ok](./images/nginx-t.png)

Khởi động lại nginx:

```plaintext
sudo systemctl restart nginx
```

`Bước 4`: Cấu hình firewall

Mở cổng HTTP (port 80):

```plaintext
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --reload
```

![config firewall](./images/conf_firewall.png)

`Bước 5`: Cấu hình file `hosts` trên máy tính cá nhân (windows)

Mở file `C:\Windows\System32\drivers\etc\hosts` bằng `Notepad` với quyền Administrator. Sau đó thêm vào cuối file:

```plaintext
192.168.133.131 censite1.com
192.168.133.131 censite2.com
```

Lưu và đóng `Notepad`.

`Bước 6`: Kiểm tra website trên trình duyệt.

```plaintext
http://censite1.com
http://censite2.com
```

Kết quả:

![test on browser](./images/censite_test.png)

### Trên Ubuntu

`Bước 1` Cài đặt Nginx trên Ubuntu Server

Trên máy ảo Ubuntu, chạy lệnh sau để cài đặt Nginx:

```plaintext
sudo apt update
sudo apt install nginx -y
```

Kiểm tra trạng thái nginx

```plaintext
sudo systemctl status nginx
```

Nếu thấy active (running), nghĩa là nginx đang chạy:

![nginx active](./images/ubuntu-active_running.png)

Nếu `inactive`, hãy khởi chạy bằng lệnh dưới, sau đó kiểm tra trạng thái lần nữa:

```plaintext
sudo systemctl start nginx 
```

`Bước 2`: Xác định địa chỉ IP của máy ảo

Chạy lệnh sau để lấy địa chỉ IP nội bộ của máy ảo:

```plaintext
ip a | grep inet
```

![ubuntu_ip](./images/ubuntu_ip.png)

Ghi nhớ địa chỉ IP này: `192.168.133.100`

`Bước 3`: Tạo thư mục cho từng website

Tạo thư mục chứa mã nguồn của `doantan1.com` và `doantan2.com`:

```plaintext
sudo mkdir -p /var/www/doantan1.com/public_html
sudo mkdir -p /var/www/doantan2.com/public_html
```

Tạo file `index.html` cho từng website:

```plaintext
echo "<h1>doantan 1 page</h1>" | sudo tee /var/www/doantan1.com/public_html/index.html

echo "<h1>doantan 2 page</h1>" | sudo tee /var/www/doantan2.com/public_html/index.html
```

Cấp quyền cho thư mục:

```plaintext
sudo chown -R www-data:www-data /var/www/doantan1.com
sudo chown -R www-data:www-data /var/www/doantan2.com
sudo chmod -R 755 /var/www
```

- `chown -R www-data:www-data`: Gán quyền sở hữu thư mục cho user www-data (user mặc định của Nginx).
- `chmod -R 755 /var/www`: Cấp quyền đọc và thực thi cho mọi user, nhưng chỉ user sở hữu mới có quyền ghi.

`Bước 4`: Cấu hình VirtualHost cho từng website

Tạo file cấu hình cho `doantan1.com`:

```plaintext
sudo vi /etc/nginx/sites-available/doantan1.com
```

Thêm nội dung sau:

```plaintext
server {
    listen 8081;
    server_name doantan1.com www.doantan1.com;
    root /var/www/doantan1.com/public_html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Tạo file cấu hình cho `doantan2.com`:

```plaintext
sudo vi /etc/nginx/sites-available/doantan1.com
```

Thêm nội dung sau:

```plaintext
server {
    listen 8081;
    server_name doantan1.com www.doantan1.com;
    root /var/www/doantan1.com/public_html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

`Bước 5`: Kích hoạt VirtualHost

Tạo liên kết `sites-available` sang `sites-enabled`

```plaintext
sudo ln -s /etc/nginx/sites-available/doantan1.com /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/doantan2.com /etc/nginx/sites-enabled/
```

- `ln -s`: Tạo liên kết mềm (symlink).
- `/etc/nginx/sites-enabled/`: Nginx chỉ load các file trong thư mục này, nên cần liên kết các file cấu hình từ `sites-available`.

Kiểm tra lại liên kết file:

```plaintext
ls -l /etc/nginx/sites-enabled/
```

![link file](./images/ubuntu-link_file.png)

`Bước 6`: Kiểm tra và restart Nginx

Chạy lệnh kiểm tra cấu hình:

```plaintext
sudo nginx -t
```

- `nginx -t`: Kiểm tra cú pháp cấu hình.

![test nginx](./images/ubuntu-test_result.png)

nếu kết quả OK, khởi động lại Nginx:

```plaintext
sudo systemctl restart nginx
```

- `systemctl restart nginx`: Khởi động lại Nginx để áp dụng thay đổi.

`Bước 7`: Cấu hình file `hosts` trên máy tính cá nhân (windows)

Mở file `C:\Windows\System32\drivers\etc\hosts` bằng `Notepad` với quyền Administrator. Sau đó thêm vào cuối file:

```plaintext
192.168.133.100 doantan1.com
192.168.133.100 doantan2.com
```

Lưu và đóng `Notepad`.

`Bước 8`: Kiểm tra website trên trình duyệt.

*Lưu ý*: Vì đã cấu hình virtualhost ở `Bước 4` với cổng 8081 nên khi truy cập, cần thêm `:8081` đằng sau URL:

```plaintext
http://doantan1.com:8081
http://doantan2.com:8081
```

Kết quả:

![test on browser](./images/ubuntu-browser_test.png)
