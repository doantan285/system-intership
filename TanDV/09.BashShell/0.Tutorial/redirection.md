# Redirection

## Hiểu về Input/Output Redirection và Pipes

Dấu `;`, ví dụ muốn di chuyển vào thư mục `/var` sau đó liệt kê file trong thư mục với lệnh `ls`

```bash
cd /var; ls
```

- Ý nghĩa: Nếu `cd /var` không lỗi -> liệt kê file trong `/var`, nếu lỗi -> liệt kê file trong thư mục hiện tại.

Dấu `&&`, chạy lệnh tiếp theo nếu lệnh trước đó thành công:

```bash
mkdir newdir && cd newdir
```

- Ý nghĩa: Nếu lệnh tạo thư mục `newdir` (ở phía trước) thành công, lệnh `cd newdir` mới được thực thi.

Dấu `||`: chạy nếu lệnh trước thất bại

```bash
mkdir newdir || echo "Không tạo được thư mục"
```

- Ý nghĩa: Nếu không tạo được thư mục -> in ra dòng "Không tạo được thư mục".

Dấu `|`: chuyền output lệnh này thành input lệnh kia

```bash
ls | grep txt
```

- ý nghĩa: liệt kê các thư mục có chữ `txt` trong tên.

Redirect: `>` (overwrite file), `>>` (append thêm vào file), `<` (đọc từ file)

```bash
echo "Hello" > file.txt
```

- `>`: chuyển output thành ghi vào file (và xóa nội dung cũ nếu có).

```bash
echo "Thêm dòng" >> file.txt
```

- `>>`: ghi thêm dòng mới vào cuối file, không xóa nội dung cũ.

```bash
wc -l < file.txt
```

- `wc -l`: đếm số dòng.
- `< file.txt`: lấy nội dung file.txt làm input cho `wc`.
- Ý nghĩa: đếm số dòng trong file `file.txt`.

`2>`: redirect lỗi

```bash
ls notfound 2> error.txt
```

- `ls notfound`: lỗi vì file không tồn tại.
- `2> error.txt`: ghi lỗi vào `error.txt` thay vì hiện ra màn hình.

## STDIN, STDOUT, STDERR, `/dev/null 2>&1`

| Tên | Số hiệu | Ý nghĩa |
|-|-|-|
| `STDIN` | 0 | Standard Input – đầu vào chuẩn |
| `STDOUT` | 1 | Standard Output – đầu ra chuẩn |
| `STDERR` | 2 | Standard Error – lỗi chuẩn |

- `/dev/nul`: giống như hố đen - mọi thứ ghi vào đây đều bị bỏ đi.

```bash
some_command > /dev/null/ 2>&1
```

- `> /dev/null`: Chuyển hướng STDOUT (1) vào `/dev/null` (bỏ đi đầu ra chuẩn).
- `2>&1`: Chuyển hướng STDERR (2) đến cùng nơi với STDOUT (1) – tức cũng bị bỏ.
