# Hello World script

## Các bước thực hiện

### 1. Tạo file script

```bash
sudo vim hello.sh
```

### 2. Thêm nội dung file (viết hàm)

```bash
#!/bin/bash

greeting () {
    echo "Hello $1!"
}

greeting World
greeting Tần
```

### 3. Cấp quyền và thực thi file

Cấp quyền thực thi:

```bash
sudo chmod +x hello.sh
```

Thực thi file:

```bash
./hello.sh
# hoặc
bash hello.sh
# hoặc
/bin/bash
```

Kết quả:

```bash
Hello World!
Hello Tần!
```
