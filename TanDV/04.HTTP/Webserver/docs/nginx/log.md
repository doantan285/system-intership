# File log Nginx

Nginx có 2 loại file log chính:

- **Access Log** (`access.log`): Ghi lại các yêu cầu HTTP mà máy chủ nhận được.
- **Error Log** (`error.log`): Ghi lại các lỗi xảy ra khi Nginx hoạt động.

## Cấu trúc file log Nginx

### 1. Access Log (`/var/log/nginx/access.log`)

File `access log` của Nginx lưu các yêu cầu HTTP với định dạng mặc định là **Combined Log Format**, bao gồm:

```plaintext
127.0.0.1 - - [20/Mar/2025:10:00:00 +0000] "GET /index.html HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
```

| Thành phần | Ý nghĩa |
|-----------|-------------|
| `127.0.0.1` | Địa chỉ IP của client gửi request |
| `-` | User identifier (thường bỏ trống) |
| `-` | Tên user (nếu có xác thực) |
| `[20/Mar/2025:10:00:00 +0000]` | Thời gian yêu cầu được gửi |
| `"GET /index.html HTTP/1.1"` | Phương thức HTTP, đường dẫn, phiên bản HTTP |
| `200` | Mã trạng thái HTTP (200: OK, 404: Not Found, 500: Internal Server Error,...) |
| `612` | Kích thước của phản hồi (bytes) |
| `-` | Referrer (nguồn trang web, nếu có) |
| `"Mozilla/5.0 (Windows NT 10.0; Win64; x64)"` | User-Agent (trình duyệt hoặc bot gửi yêu cầu) |

### 2. Error Log (`/var/log/nginx/error.log`)

File `error.log` ghi lại các lỗi xảy ra khi Nginx hoạt động. Mức độ log có thể là `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, `emerg`.

```plaintext
2025/03/20 10:05:23 [error] 1234#1234: *1 open() "/usr/share/nginx/html/missing.html" failed (2: No such file or directory), client: 192.168.1.100, server: localhost, request: "GET /missing.html HTTP/1.1", host: "example.com"
```

| Thành phần | Ý nghĩa |
|-----------|-------------|
| `2025/03/20 10:05:23` | Thời gian xảy ra lỗi |
| `[error]` | Mức độ lỗi |
| `1234#1234` | ID tiến trình Nginx |
| `open() "/usr/share/nginx/html/missing.html" failed (2: No such file or directory)` | Mô tả lỗi |
| `client: 192.168.1.100` | Địa chỉ IP của client gửi request |
| `server: localhost` | Máy chủ được truy vấn |
| `"GET /missing.html HTTP/1.1"` | Yêu cầu HTTP |

**Các mức độ log (error_log):**

| **Mức độ** | **Mô tả** | **Ví dụ lỗi** |
|-----------|-------------|---------|
| `debug` | Ghi lại tất cả thông tin chi tiết, thường chỉ dùng để debug lỗi. Rất nhiều log. | Kết nối đến backend, proxy, cache, quá trình xử lý request. |
| `info` | Thông tin chung về quá trình hoạt động, không phải lỗi. | Server khởi động thành công, tải lại cấu hình. |
| `notice` | Thông báo quan trọng nhưng không gây gián đoạn dịch vụ. | Một tính năng bị vô hiệu hóa, Nginx bỏ qua một tùy chọn. |
| `warn` | Cảnh báo có thể gây lỗi trong tương lai nhưng chưa gây gián đoạn ngay. | Quá nhiều request từ một IP, bộ nhớ cache sắp đầy. |
| `error` | Lỗi xảy ra nhưng Nginx vẫn tiếp tục chạy. Đây là mức phổ biến nhất. | File cấu hình sai, file không tồn tại, lỗi truy cập file. |
| `crit` | Lỗi nghiêm trọng, có thể ảnh hưởng đến dịch vụ. | Không thể kết nối đến backend, tài nguyên bị thiếu. |
| `alert` | Lỗi rất nghiêm trọng, cần xử lý ngay lập tức. | Lỗi phân quyền, lỗi nghiêm trọng khi ghi file. |
| `emerg` | Lỗi cấp độ khẩn cấp, Nginx không thể tiếp tục chạy. | Port bị chiếm, cấu hình lỗi nặng khiến Nginx không khởi động được. |

## So sánh log của Nginx và Apache

| **Tiêu chí** | **Nginx** (`access.log`, `error.log`) | **Apache** (`access.log`, `error.log`) |
|-----------|-------------|---------|
| **Vị trí log mặc định** | `/var/log/nginx/` | `/var/log/httpd/` (CentOS) / /`var/log/apache2/` (Ubuntu) |
| **Định dạng mặc định** | Combined Log Format | Common Log Format (có thể dùng Combined) |
| **Access Log mặc định** | `/var/log/nginx/access.log` | `/var/log/httpd/access_log` hoặc `/var/log/apache2/access.log` |
| **Error Log mặc định** | /var/log/nginx/error.log | `/var/log/httpd/error_log` hoặc `/var/log/apache2/error.log` |
| **Mức độ error log** | `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, `emerg` | `debug`, `info`, `notice`, `warn`, `error`, `crit`, `alert`, `emerg` |
| **Cấu trúc log** | Ghi theo từng dòng, đơn giản hơn | Có thêm lỗi từ module Apache (VD: mod_rewrite, mod_ssl) |
| **Hiệu suất ghi log** | Ghi log nhanh hơn do kiến trúc non-blocking | Có thể bị chậm hơn khi ghi log nhiều request cùng lúc |

## Kiểm tra log và debug lỗi

Kiểm tra log real-time:

```plaintext
sudo tail -f /var/log/nginx/access.log
```

Kiểm tra lỗi:

```plaintext
sudo tail -f /var/log/nginx/error.log
```

Lọc lỗi `error` hoặc `warn`:

```plaintext
grep 'error' /var/log/nginx/error.log
grep 'warn' /var/log/nginx/error.log
```
