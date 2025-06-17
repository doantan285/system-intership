# Câu lệnh điều kiện

##

### 1. Cấu trúc `if`, `elif`, `else`

```bash
if [ điều_kiện ]; then
  # khối lệnh nếu điều kiện đúng
elif [ điều_kiện_khác ]; then
  # khối lệnh nếu điều kiện khác đúng
else
  # khối lệnh nếu không điều kiện nào đúng
fi
```

- `then`: bắt đầu khối lệnh
- `fi`: Kết thúc khối lệnh if

Ví dụ:

```bash
#!/bin/bash
echo "Nhập tuổi:"
read age

if [ $age -lt 18 ]; then
  echo "Chưa đủ tuổi."
elif [ $age -lt 65 ]; then
  echo "Trưởng thành."
else
  echo "Nghỉ hưu."
fi
```

- Khi chạy, script sẽ hỏi tuổi và phân loại bạn.

### 2. Cấu trúc `Case`

```bash
case $var in
  giá_trị1)
    # lệnh nếu khớp giá trị1
    ;;
  giá_trị2)
    # lệnh nếu khớp giá trị2
    ;;
  *)
    # lệnh nếu không khớp giá trị nào
    ;;
esac
```

- `case` ... `in` để bắt đầu
- `;;` để kết thúc từng trường hợp
- `*` là mặc định (else)

Ví dụ:

```bash
#!/bin/bash

echo "Nhập một lựa chọn (start, stop, restart):"
read action

case $action in
  start)
    echo "Đang khởi động dịch vụ..."
    ;;
  stop)
    echo "Đang dừng dịch vụ..."
    ;;
  restart)
    echo "Đang khởi động lại dịch vụ..."
    ;;
  *)
    echo "Lựa chọn không hợp lệ!"
    ;;
esac
```

- In ra "Hệ điều hành Linux."
