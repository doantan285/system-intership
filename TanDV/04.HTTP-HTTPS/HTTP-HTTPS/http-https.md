# Tìm hiểu HTTP/HTTPS

HTTP (Hypertext Transfer Protocol) và HTTPS (Hypertext Transfer Protocol Secure) là hai giao thức nền tảng cho việc truyền tải dữ liệu trên World Wide Web.

## HTTP

### Khái niệm HTTP

Giao thức HTTP (HyperText Transfer Protocol) là một giao thức lớp ứng dụng trong mô hình OSI (Open Systems Interconnection) và là nền tảng cơ bản cho việc trao đổi thông tin giữa máy khách (client) và máy chủ (server) qua internet.

HTTP định nghĩa các loại yêu cầu và phản hồi giữa máy khách và máy chủ, giúp World Wide Web hoạt động một cách nhất quán và hiệu quả. Giao thức HTTP sử dụng mô Hình Client-Server:

- **Máy khách**: Là thiết bị hoặc phần mềm, chẳng hạn như trình duyệt web, gửi yêu cầu tới máy chủ.
- **Máy chủ**:  Là hệ thống lưu trữ các tài nguyên web và phản hồi yêu cầu từ máy khách.

### Cách hoạt động của HTTP

`Bước 1: Client khởi tạo yêu cầu`

- **Nhập địa chỉ Web và gửi yêu cầu HTTP:** Nhập một URL vào trình duyệt và nhấn Enter, trình duyệt tạo một lệnh HTTP yêu cầu máy chủ tìm và gửi nội dung của trang web tương ứng với địa chỉ đã nhập. Yêu cầu này được gửi đến máy chủ qua giao thức HTTP và có thể bao gồm:
  - **Phương Thức:** Chẳng hạn như GET (để lấy dữ liệu) hoặc PUT (để gửi dữ liệu).
  - **URL:** Địa chỉ của tài nguyên cần truy cập.
  - **Headers:** Thông tin bổ sung như loại trình duyệt, ngôn ngữ chấp nhận.
  - **Body:** Dữ liệu gửi đi, chỉ có trong các yêu cầu PUT hoặc POST.

`Bước 2: Máy chủ nhận và xử lý yêu cầu`

- Máy chủ nhận yêu cầu và xử lý nó. Máy chủ tìm kiếm tài nguyên, thực hiện các truy vấn cơ sở dữ liệu nếu cần, và chuẩn bị phản hồi.

`Bước 3: Máy chủ gửi phản hồi HTTP về trình duyệt`, gồm:

- **Mã Trạng Thái:**
  - 200 OK: Yêu cầu thành công và máy chủ trả lại nội dung yêu cầu.
  - 400 Bad Request: Yêu cầu bị lỗi và không thể được xử lý.
  - 404 Not Found: Tài nguyên yêu cầu không được tìm thấy trên máy chủ.
  - 500 Internal Server Error – Lỗi máy chủ nội bộ.
- **Headers:** Thông tin về phản hồi như loại nội dung và chiều dài.
- **Body:** Nội dung thực tế của phản hồi, chẳng hạn như mã HTML, CSS, hoặc dữ liệu JSON.

`Bước 4: Hiển thị nội dung`

- Trình duyệt nhận phản hồi từ máy chủ, giải mã và hiển thị trang web.

`Bước 5: Kết thúc kết nối`

- Sau khi dữ liệu được truyền tải hoàn tất, kết nối có thể được đóng. Tuy nhiên, với các phiên bản mới hơn của HTTP, kết nối có thể được giữ mở để tái sử dụng cho các yêu cầu tiếp theo.

### Các phương phương thức (Methods) chính trong HTTP

| Phương thức | Chức năng |
|-----------|-------------|
| GET | Lấy dữ liệu từ máy chủ. |
| POST | Gửi dữ liệu lên máy chủ (ví dụ: form đăng ký). |
| PUT | Cập nhật toàn bộ dữ liệu tài nguyên. |
| PATH | Cập nhật một phần dữ liệu tài nguyên. |
| DELETE | Xóa tài nguyên trên máy chủ. |
| HEAD | Giống GET nhưng chỉ trả về phần header. |
| OPTIONS | Kiểm tra các phương thức được hỗ trợ. |

## HTTPS

### Khái niệm HTTPS

### Cách hoạt động của HTTPS

## Khác nhau giữa HTTP và HTTPS
