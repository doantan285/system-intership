# Hướng dẫn học Bash Shell

## Các Khái niệm cơ bản

### 1. Khác nhau giữa Terminal và Shell

**Terminal**: là cửa sổ giao diện để giao tiếp với máy tính thông qua văn bản, Nó giống như cái màn hình để gõ lệnh vào và nhìn thấy kết quả.

- Ví dụ: Mở ứng dụng **Terminal** trên **Ubuntu**, **macOS**, hoặc **Command Prompt** trên **Windows** → đó là **Terminal**. **Terminal** không xử lý lệnh. Nó chỉ nhận lệnh gõ vào rồi chuyển cho shell xử lý.

**Shell**: là chương trình đọc lệnh người dùng gõ trong **Terminal**, hiểu lệnh, và thực thi lệnh đó. Nó thông dịch lệnh gõ, làm việc với hệ điều hành để cho ra kết quả.

- Ví dụ: **Shell** phổ biến nhất là **Bash** (`/bin/bash`). Ngoài **Bash**, còn có các shell khác như: **Zsh**, **Fish**, **Sh**, **Ksh**...

### 2. Cấu trúc dòng lệnh

```bash
command [options] [arguments]
```

- `command`: tên lệnh (ví dụ: ls, cp, cat, echo,...)
- `[options]`: (không bắt buộc) các tùy chọn để thay đổi cách lệnh hoạt động (thường bắt đầu bằng `-` hoặc `--`)
- `[arguments]`: (không bắt buộc) đối tượng mà lệnh sẽ xử lý (ví dụ: tên file, thư mục...)

Ví dụ:

```bash
ls -l /home/username
```

- `ls`: lệnh liệt kê file.
- `-l`: option hiển thị chi tiết.
- `/home/username`: argument chỉ định thư mục cần liệt kê.

### 3. Đường dẫn

- **Tuyệt đối:** bắt đầu từ `/`, ví dụ `/home/user/docs/file.txt`
- **Tương đối:** tính từ vị trí hiện tại, ví dụ `../docs/file.txt`
- Ví dụ:
  - cd `/etc` (tuyệt đối)
  - cd `../folder` (tương đối)

## Một số câu lệnh khác

| Lệnh | Ý nghĩa | Ví dụ |
|-----------|-------------|---------|
| `tar` | Nén/giải nén file `.tar` | `tar -cvf archive.tar file1 file2` |
| `gzip` / `gunzip` | Nén/giải nén file `.gz` | `gzip file.txt` |
| `zip` / `unzip` | Nén/giải nén file `.zip` | `zip archive.zip file1 file2` |
| `ping` | Kiểm tra kết nối tới host | `ping google.com` |
| `ifconfig` hoặc `ip addr` | Xem địa chỉ IP | `ip adrr` |
| `curl` | Gửi yêu cầu HTTP | `curl https://example.com` |

## Bash scripting

### 1. Tạo scrip với shebang

Ví dụ file `myscript.sh`:

```bash
#!/bin/bash
echo "Hello from my first script!"
```

- `#!/bin/bash`: Shebang, chỉ ra file này dùng Bash để chạy.
- `echo`: In ra dòng chữ.

### 2. Biến

In ra dòng "Hi Jhon":

```bash
name="John"
echo "Hi $name"
```

- `name="Jhon"`: khai báo biến.
- `$name`: lấy giá trị biến.

### 3. Tham số dòng lệnh

```bash
echo "Script name: $0"
echo "First agurment: $1"
echo "Second agurment: $2"
```

- `$0`: Tên script.
- `$1`, `$2`: Các tham số truyền vào.

Ví dụ chạy:

```bash
./myscript.sh file1 file2
```

Output:

```yaml
Script name: ./myscript.sh
First argument: file1
Second argument: file2
```
