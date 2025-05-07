# Vòng lặp (Loops)

##

### 1. Vòng lặp `for`

```bash
for var in list; do
    # Khối lệnh
done
```

Ví dụ:

```bash
#!/bin/bash
for name in Alice Bob Charlie; do
    echo "Hello $name"
done
```

Output

```yaml
Hello Alice
Hello Bob
Hello Charlie
```

### 2. Vòng lặp `while`

```bash
while [ condition ]; do
    #Khối lệnh
done
```

Ví dụ:

```bash
#!/bin/bash
count=1
while [ $count -le 5 ]; do
    echo "Lần lặp: $count"
    ((count++))
done
```

Output:

```yaml
Lần lặp: 1
Lần lặp: 2
Lần lặp: 3
Lần lặp: 4
Lần lặp: 5
```

### 3. vòng lặp `until`

```bash
until [ condition ]; do
    # Khối lệnh
done
```

- `until` chạy cho tới khi điều kiện đúng.

Ví dụ:

```bash
#!/bin/bash
until [ $count -gt 5 ]; do
    echo "Lần lặp $count"
    ((count++))
done
```

- `until` giống `while ! [ điều kiện ]`.
