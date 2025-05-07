# Bash Shell

## Hàm (functions)

### 1. Định nghĩa hàm

```bash
function_name() {
    # Khối lệnh
}
```

hoặc

```bash
function function_name {
    # Khối lệnh
}
```

### 2.Ví dụ dùng hàm

```bash
#!/bin/bash

say_hello() {
    echo "Hello $1"
}

say_hello World
```

Output:

```yaml
Hello World
```

## Xử lý kiểm tra lỗi (Error Handling)

### 1. Kiểm tra mã thoát `$?`

- Sau mỗi lệnh, bash lưu mã thoát trong `$?`.
- Nếu $? = 0: lệnh thành công.
- Nếu $? ≠ 0: lệnh thất bại.

Ví dụ:

```bash
mkdir newfolder
if [ $? -eq 0 ]; then
    echo "Tạo thư mục thành công."
else
    echo "Tạo thư mục thất bại."
fi
```

### 2. Kiểm tra nhanh gọn hơn

Sử dụng `&&` và `||`:

```bash
mkdir newfolder && echo "Tạo folder thành công." || echo "Tạo folder thất bại."
```
