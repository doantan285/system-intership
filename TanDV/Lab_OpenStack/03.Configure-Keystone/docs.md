# Configure Keystone

## 1. Thêm user và database trên mariadb cho keystone

**Chạy giao diện dòng lệnh mysql:**

```bash
sudo mysql -u root -p
```

**Tạo database tên là `keystone`:**

```sql
CREATE DATABASE keystone;
```

**Cấp toàn quyền (ALL PRIVILEGES) cho user `keystone` trên database `keystone`:**

```sql
grant all privileges on keystone.* to 'keystone'@'localhost' identified by 'kspassword';
```

- `@localhost`: Chỉ cho phép kết nối từ localhost.
- `kspassword`: Mật khẩu cho user `keystone`.

**Cấp quyền tương tự những cho phép user `keystone` kết nối từ bất kỳ địa chỉ IP nào:**

```sql
grant all privileges on keystone.* to 'keystone'@'%' identified by 'kspassword';
```

**Tải lại bảng phân quyền của mariadb để áp dụng các thay đổi mà không cần khởi động lại dịch vụ:**

```sql
FLUSH PRIVILEGES;
```

**Thoát khỏi giao diện dòng lệnh mysql:**

```sql
exit;
```

## 2. Cài đặt Keystone

```bash
sudo apt -y install keystone python3-openstackclient apache2 libapache2-mod-wsgi-py3 python3-oauth2client
```

- **keystone:** Cung cấp xác thực (authentication) và ủy quyền (authorization) cho toàn bộ hệ thống OpenStack. Gói này chứa các thành phần chính của Keystone như cấu hình, dịch vụ WSGI, tool quản lý như `keystone-manage`, script khởi tạo CSDL…
- **python3-openstackclient:** CLI chính để quản trị tất cả các dịch vụ OpenStack (Keystone, Glance, Nova, Neutron...). Lệnh sử dụng: `openstack <command>`.
- **apache2:** Keystone hoạt động dưới dạng WSGI application, nên cần một web server để phục vụ nó. Apache2 là lựa chọn phổ biến trong các bản hướng dẫn chính thức của OpenStack.
- **libapache2-mod-wsgi-py3:** module kết nối giữa Apache2 và ứng dụng Python. Keystone là ứng dụng Python WSGI, vì vậy module này cho phép Apache thực thi Keystone WSGI thông qua các file như `keystone-wsgi-public`.
- **python3-oauth2client:** thư viện Python giúp Keystone hỗ trợ OAuth 2.0 nếu bạn cần tích hợp xác thực theo chuẩn này (ví dụ: kết nối với Google, GitHub, Azure AD...). Mặc định Keystone không bắt buộc dùng OAuth2, nhưng gói này được cài thêm cho các môi trường có nhu cầu.

## 3. Cấu hình keystone

**Mở file cấu hình keystone:**

```bash
sudo vim /etc/keystone/keystone.conf
```

**Tìm và chỉnh sửa các dòng sau:**

```bash
# line 444 : add to specify Memcache Server
memcache_servers = 192.168.133.133:11211
# line 661 : change to MariaDB connection info
connection = mysql+pymysql://keystone:kspassword@192.168.133.133/keystone
# line 2639 : uncomment
provider = fernet
```

**Khởi tạo cơ sở dữ liệu cho Keystone:**

```bash
sudo su -s /bin/bash keystone -c "keystone-manage db_sync"
```

- `su -s /bin/bash keystone`: chạy lệnh dưới quyền user keystone (không phải root).
- `keystone-manage db_sync` để:
  - Tạo tất cả bảng dữ liệu cho `keystone`.
  - Áp dụng các lược đồ (schema) mới nhất.
  - Không xóa dữ liệu cũ (nếu có).

>Lệnh này bắt buộc phải chạy sau khi tạo DB, nếu không keystone sẽ không chạy do không có bảng dữ liệu.

**Khởi tạo Fernet key:**

```bash
sudo keystone-manage fernet_setup --keystone-user keystone --keystone-group keystone
```

- Tạo ra các fernet key dùng để mã hóa/giải mã token trong Keystone.
- `Fernet` là loại token nhẹ, không lưu trong database (stateless), được sử dụng mặc định từ các phiên bản Keystone mới.
- Cờ `--keystone-user` và `--keystone-group` chỉ định user/group sở hữu file key để đảm bảo quyền truy cập đúng.

  >Các key này được lưu trong thư mục `/etc/keystone/fernet-keys/`.

**Khởi tạo credential key:**

```bash
sudo keystone-manage credential_setup --keystone-user keystone --keystone-group keystone
```

- Tạo ra các credential key dùng để mã hóa thông tin nhạy cảm như: tài khoản EC2 credentials, certificate.
- Được sử dụng để bảo vệ dữ liệu người dùng mà keystone lưu trữ.

  >Các key này được lưu trong thư mục `/etc/keystone/credential-keys/`.

**Đặt biến môi trường `controller`:**

```bash
export controller=192.168.133.133
```

- Thiết lập biến môi trường `$controller` để có thể tái sử dụng trong lệnh tiếp theo.
- Hostname của máy là server, Máy chưa có FQDN đúng nghĩa vì chưa gắn với domain nào (chỉ có tên rút gọn).

  >Không thể dùng server như một FQDN thực sự trong môi trường OpenStack nhiều node (nhất là khi cấu hình Keystone, Glance...).

**Bootstrap keystone:** khởi tạo  dịch vụ xác thực keystone trong OpenStack., thiết lập cấu hình ban đầu cho hệ thống quản lý danh tính (Identity Service).

```bash
keystone-manage bootstrap --bootstrap-password adminpw --bootstrap-admin-url https://$controller:5000/v3/ --bootstrap-internal-url https://$controller:5000/v3/ --bootstrap-public-url https://$controller:5000/v3/ --bootstrap-region-id RegionOne
```

- `keystone-manage bootstrap`: tạo các thực thể cơ bản trong keystone: user admin, project admin, role admin, và endpoint API ở các mức (admin, internal, public).
- `--bootstrap-password adminpw`: Gán mật khẩu cho user admin (quản trị viên đầu tiên).
- `--bootstrap-admin-url`: Tạo endpoint dành riêng cho admin (thường dùng cho lệnh CLI hoặc script backend).
- `--bootstrap-internal-url`:Tạo endpoint cho các dịch vụ nội bộ (internal) giao tiếp với Keystone.
- `--bootstrap-public-url`: Tạo endpoint công khai (public) cho người dùng và dashboard truy cập.
- `--bootstrap-region-id RegionOne`: Tên region đầu tiên trong hệ thống OpenStack. Mặc định là RegionOne.

## 4. Tạo chứng chỉ SSL tự ký

### 4.1 Cấu hình tên miền giả (lab nội bộ)

Mở cấu hình file hosts:

```bash
sudo vim /etc/hosts
```

Thêm dòng sau vào file:

```bash
192.168.133.133 dlp.lab.local
```

### 4.2 Thêm cấu hình SAN (Subject Alternative Name) vào `/etc/ssl/openssl.cnf`

Mở file cấu hình OpenSSL:

```bash
sudo vim /etc/ssl/openssl.cnf
```

Thêm vào cuối file:

```bash
[ lab.local ]
subjectAltName = DNS:dlp.lab.local
```

### 4.3 Di chuyển vào thư mục chứa khóa riêng

```bash
cd /etc/ssl/private
```

> Thư mục này thường được bảo vệ (`chmod 700`), chỉ root mới có quyền — để đảm bảo an toàn cho private key.

### 4.4 Tạo khóa riêng (private key)

```bash
sudo openssl ecparam -name prime256v1 -genkey -out server.key
```

- Tạo private key dùng thuật toán **ECC (Elliptic Curve)** thay vì **RSA** (nhẹ hơn, bảo mật hơn).
- `prime256v1`: curve phổ biến tương đương RSA 3072-bit.
- Kết quả: file `server.key` chứa private key.

### 4.5 Tạo Certificate Signing Request (CSR)

```bash
sudo openssl req -new -key server.key -out server.csr
```

| Trường | Nội dung nhập |
|--------|---------|
| Country Name | `VN` |
| State or Province Name | `Hanoi` |
| Locality Name | `Hanoi` |
| Organization Name | `VNPT-IT` |
| Organizational Unit Name | `Cloud` |
| Common Name | `dlp.lab.local` |
| Email Address | `doantan1802@gmail.com` |

### 4.6 Tạo chứng chỉ tự ký

```bash
openssl x509 -in server.csr -out server.crt -req -signkey server.key -extfile /etc/ssl/openssl.cnf -extensions lab.local -days 3650
```

- `-signkey`: ký bằng private key (`server.key`).
- `-extfile`: chỉ định file chứa thông tin `subjectAltName`.
- `-days 3650`: chứng chỉ có hạn 10 năm.

>`lab.local` trong `-extensions lab.local` phải trùng với section `[ lab.local ]` trong `openssl.cnf`.

Kiểm tra kết quả với lệnh `ls -l /etc/ssl/private/`:

```bash
-rw-r--r-- 1 root root      871 Jun 30 22:54 server.crt # chứng chỉ
-rw-r--r-- 1 root root      525 Jun 30 22:49 server.csr # yêu cầu chứng chỉ
-rw------- 1 root root      302 Jun 30 22:46 server.key # khóa riêng
```

## 5. Cấu hình Apache httpd

### 5.1 Mở file cấu hình chính

```bash
sudo vim /etc/apache2/apache2.conf
```

Line 70: chỉ định tên máy chủ

```apache
ServerName dlp.lab.local
```

- Tránh cảnh báo lỗi khi khởi động Apache:`.Could not reliably determine the server's fully qualified domain name`.
- `dlp.lab.local` là tên máy (hostname) dùng trong `/etc/hosts` và SSL.

### 5.2 Tạo file cấu hình VirtualHost cho Keystone

```bash
sudo vim /etc/apache2/sites-available/keystone.conf
```

Thêm nội dung sau vào dưới dòng `<VirtualHost *:5000>`

```apache
ServerName dlp.lab.local
    SSLEngine on
    SSLHonorCipherOrder on
    SSLCertificateFile /etc/ssl/private/server.crt
    SSLCertificateKeyFile /etc/ssl/private/server.key
```

### 5.3 Kích hoạt các module SSL và WSGI và khởi động lại Apache

```bash
sudo a2enmod ssl
sudo a2enmod wsgi
sudo systemctl restart apache2
```

- Apache cần `mod_ssl` để xử lý HTTPS và `mod_wsgi` để chạy ứng dụng Python như Keystone.

### 5.4 Kiểm tra truy cập bằng `curl`

```bash
curl -k https://dlp.lab.local:5000/
```

- `-k`: Bỏ qua xác thực chứng chỉ SSL (vì đây là chứng chỉ tự ký).
- Nếu hiển thị JSON là hoạt động tốt.

## 6. Thêm projects trong Keystone

### 6.1 Tải các biến môi trường

```bash
root@server:~# vim ~/keystonerc

# Thêm nội dung sau vào file keystonerc
export OS_PROJECT_DOMAIN_NAME=Default
export OS_USER_DOMAIN_NAME=Default
export OS_PROJECT_NAME=admin
export OS_USERNAME=admin
export OS_PASSWORD=kspassword
export OS_AUTH_URL=https://dlp.lab.local:5000/v3
export OS_IDENTITY_API_VERSION=3
export OS_IMAGE_API_VERSION=2
export PS1='\u@\h \W(keystone)\$ '
```

- `OS_PROJECT_DOMAIN_NAME`: Domain chứa project `admin` (mặc định là `Default`).
- `OS_USER_DOMAIN_NAME`: Domain chứa user `admin` (mặc định là `Default`).
- `OS_PROJECT_NAME`: project mà bạn login vào (`admin`)
- `OS_USERNAME`: User `admin` (Tạo khi bootstrap keystone).
- `OS_PASSWORD`: Mật khẩu đã dùng khi `keystone-manage bootstrap`:`kspassword`.
- `OS_AUTH_URL`: API endpoint của Keystone, dùng hostname đã đặt (`dlp.lab.local`)
- `OS_IDENTITY_API_VERSION`: Keystone API v3
- `OS_IMAGE_API_VERSION`: Phiên bản API của Glance (khi cài sau).
- `PS1`: Thay đổi dòng prompt để nhận biết môi trường keystone đang bật.

```bash
# Phân quyền bảo mật: bảo vệ file vì chứa mật khẩu
chmod 600 ~/keystonerc

# Tải vào shell hiện tại để dùng lệnh openstack mà không cần nhập thêm mật khẩu
source ~/keystonerc

# Tự động load khi mở terminal
echo "source ~/keystonerc" >> ~/.bashrc
```

### 6.2 Tạo project `service`

Projet `service` là nơi chứa các dịch vụ OpenStack (như nova, glance, neutron...)

```bash
openstack project create --domain default --description "Service Project" service --insecure
```

- Thêm `--insecure` vì đang dùng chứng chỉ tự ký.

Kết quả khi thành công:

![Service project created](./images/create_service_project.png)

### 6.3 Kiểm tra project đã tạo

```bash
openstack project list --insecure
```

![Projects list](./images/projects_list.png)
