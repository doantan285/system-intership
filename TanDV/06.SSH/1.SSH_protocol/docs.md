# Tổng quan về SSH

## SSH là gì?

SSH là một giao thức bảo mật phổ biến trong lĩnh vực CNTT. Nhờ được sử dụng rộng rãi trong việc quản lý từ xa và truyền tải dữ liệu một cách an toàn, SSH đặt ra tiêu chuẩn cao về đảm bảo thông tin, ngăn chặn các rủi ro an ninh mạng.

![What is SSH?](./images/what-is-ssh.jpg)

### 1. Khái niệm SSH

**SSH** là viết tắt của Secure Shell, là một giao thức mạng mã hóa cho phép bạn thiết lập một kết nối an toàn giữa hai thiết bị. Nó thường được sử dụng để truy cập và quản lý các máy chủ từ xa một cách an toàn qua mạng không an toàn như **Internet**.

### 2. Tính năng chính

- **Truy cập máy chủ từ xa:** Cho phép quản trị viên kết nối và điều khiển máy chủ Linux/Unix từ xa.
- **Truyền tệp an toàn (Secure File Transfer):** Cho phép truyền dữ liệu an toàn qua các kết nối không an toàn bằng cách bọc dữ liệu trong đường hầm SSH. Có 2 loại:
  - **Local Port Forwarding:** Định tuyến lưu lượng từ máy cục bộ qua máy chủ SSH.
  - **Remote Port Forwarding:** Cho phép máy chủ SSH chuyển tiếp lưu lượng đến một máy khác.
- **SCP & SFTP:**
  - `scp` (Secure Copy): Truyền tệp tin giữa client và server an toàn.
  - `sftp` (SSH File Transfer Protocol): Giao thức truyền tệp an toàn dựa trên SSH.
  - vd: truyền file từ ubuntu sang centos:
  
    ```plaintext
    scp text.txt doantan1@192.168.3.88:/home/doantan1/
    hoặc chỉ cho kết nối với keypair
    scp -i ~/.ssh/id_rsa text.txt doantan1@192.168.8.88:/home/doantan1/
    ```

- **Chạy lệnh từ xa:** Có thể thực thi lệnh trên máy chủ mà không cần mở phiên shell (`ssh user@server "ls -l"`).

## Cách hoạt động của SSH

![ssh working](./images/ssh_work.png)

### Giai đoạn 1 - TCP handshake

- Client khởi tạo kết nối TCP đến server qua cổng mặc định 22 (hoặc cổng tùy chỉnh).
- Server lắng nghe trên cổng 22 và phản hồi bằng một gói tin SYN-ACK để chấp nhận yêu cầu kết nối.
- Client gửi lại một gói tin ACK để hoàn tất quá trình bắt tay ba bước (three-way handshake) của TCP. Lúc này, một kết nối TCP đã được thiết lập giữa client và server.

### Giai đoạn 2 - Protocol Version Exchange

- Sau khi kết nối TCP được thiết lập, client và server bắt đầu thương lượng về phiên bản giao thức SSH mà họ sẽ sử dụng và các thuật toán mã hóa, xác thực được hỗ trợ, ví dụ:

```plaintext
SSH-2.0-OpenSSH_9.3p1 Ubuntu-1
```

### Giai đoạn 3 - Key Exchange

- Mục tiêu của giai đoạn này là tạo ra **Session key**. Quá trình này thường sử dụng các thuật toán trao đổi khóa như Diffie-Hellman (DH) hoặc Elliptic Curve Diffie-Hellman (ECDH).
- Các bước cơ bản (ví dụ sử dụng Diffie-Hellman (DH)):
  - Cả hai bên chọn một cặp số chung: `g` (generator), `p` (số nguyên tố lớn).
  - Mỗi bên chọn một số bí mật: client chọn `a`, server chọn `b`.
  - Tính public value: Client tính `A = g^a mod p`, Server tính `B = g^b mod p`.
  - Client gửi `A` cho server, và server gửi `B` cho client.
  - Client tính `K = B^a mod p`; Server tính `K = A^b mod p`.
  - **Session key** = K + random + session ID.
- Ví dụ:
  - Giả sử `g = 5`, `p = 23`
  - Client chọn `a = 6` → `A = 5^6 mod 23 = 8`
  - Server chọn `b = 15` → `B = 5^15 mod 23 = 2`
  - Client nhận `B = 2` → tính `K = 2^6 mod 23 = 18`
  - Server nhận `A = 8` → tính `K = 8^15 mod 23 = 18`
  - Kết quả: cùng shared secret `K = 18` mà không cần gửi a hoặc b

### Giai đoạn 4 - User Authentication

![ssh key authentication](./images/ssh_key_auth.png)

- Để đảm bảo rằng client đang kết nối đúng đến server mong muốn và không phải là một kẻ tấn công trung gian (man-in-the-middle attack), client cần xác minh danh tính của server.
- Client tạo keypair (gồm public key và private key) và gửi public key cho server.

  ```plaintext
  ssh-copy-id -i ~/.ssh/id_rsa.pub doantan1@192.168.3.73
  ```

- public key được lưu trong tệp `~/.ssh/authorized_keys` của server
- Xác thực bằng keypair:
  - Server gửi một chuỗi thử thách (challenge) – có thể là số ngẫu nhiên, session ID, hoặc 1 message do server chọn.
  - Client ký số chuỗi đó bằng private key của mình.
  - Server dùng public key (client đã đăng ký trước) để kiểm tra chữ ký.
  - Nếu chữ ký hợp lệ → xác thực thành công

### Giai đoạn 5 - Request Services (Sau xác thực)

Sau khi xác thực thành công, client có thể yêu cầu dịch vụ.
