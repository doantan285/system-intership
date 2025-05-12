# Viết script thông báo sinh nhật

## 1. File dữ liệu sinh nhật

Tạo file `birthdays.txt` với định dạng `<Tên> <MM-DD-YYYY>`:

```sql
Doan Tan 06-05
Thanh Binh 07-05
Son Nguyen 08-05
Van Quyen 09-05
```

## 2. Script kiểm tra và thông báo sinh nhật

Tạo file `birthday_alert_bot.sh`

```bash
#!/bin/bash

# Bot và Channel information
BOT_TOKEN="8002836179:AAGCCjbWp9hbM2rj0xTEFykYQNa0vlxaBoE"
CHANNEL_ID="@your_channel_username"  # Hoặc dùng ID: -100xxxxxxxxxx

# Today (DD-MM)
TODAY=$(date +%d-%m)

# Path to birthday data file
BIRTHDAY_FILE="/home/ubuntu/scripts/birthdays.txt"

# Iterate through each line in the file
while IFS= read -r line; do
  # Skip empty lines or comment lines
  [[ -z "$line" || "$line" =~ ^# ]] && continue

  # Separate name and birthday
  NAME=$(echo "$line" | awk '{$NF=""; print $0}' | sed 's/ *$//')
  BDAY=$(echo "$line" | awk '{print $NF}')

  # If the format is valid and matches today
  if [[ "$BDAY" == "$TODAY" ]]; then
    read -r -d '' MESSAGE <<EOF
    *Birthday Alert!*
    Today is *$NAME*'s birthday ($TODAY)
    EOF

    curl -s -X POST "https://api.telegram.org/bot$BOT_TOKEN/sendMessage" \
      -d chat_id="$CHANNEL_ID" \
      --data-urlencode text="$MESSAGE" \
      -d parse_mode="Markdown"
  fi
done < "$BIRTHDAY_FILE"
```

## 3. Cấp quyền và thiết lập crontab

Cấp quyền thực thi:

```shell
sudo chmod +x birthday_alert_bot.sh
```

Mở cron:

```shell
crontab -e
```

Thêm nội dung dưới đây để chạy mỗi 8h sáng:

```shell
0 8 * * * /home/ubuntu/scripts/birthday_alert_bot.sh
```

## 4. Kết quả

Chạy thử script bằng lệnh `./birthday_alert_bot.sh`, kết quả:

![birthday alert](./images/birthday_alert.png)
