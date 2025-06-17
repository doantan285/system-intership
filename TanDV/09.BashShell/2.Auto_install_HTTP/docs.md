# Script tự động cài HTTP

## Mục tiêu

1. Kiểm tra quyền `sudo`.
2. Cập nhật hệ thống.
3. Cài đặt Apache.
4. Kiểm tra dịch vụ.
5. Hiển thị địa chỉ IP để truy cập web

## Nội dung file `install_http.sh`

```bash
#!/bin/bash

# Check not root or not using sudo
# [[ $EUID -ne 0 ]] && echo "Please run with sudo" && exit 1
if [[ $EUID -ne 0 ]]; then
    echo "Please run the script with sudo or root privileges"
    exit 1
fi

echo "updating system..."
sudo apt update -y && apt upgrade -y

echo "installing Apache HTTP Server..."
sudo apt install apache2 -y

echo "enable and start Apache service..."
sudo systemctl enable apache2
sudo systemctl start apache2

echo "Checking Apache service status..."
sudo systemctl is-active apache2

# Check if port 80 is open (Apache use it)
echo "Checking port 80..."
if sudo ss -tuln | grep ':80' > /dev/null; then
    echo "Port 80 is open and Apache is watching."
else
    echo "Port 80 not open. Maybe Apache is not running properly."
fi

# Install and configure ufw firewall
echo "installing ufw and opening port 80 HTTP..."
sudo apt install ufw -y
ufw allow 80/tcp
ufw allow 'Apache'
ufw enable <<EOF
y
EOF

echo "Current ufw status:"
sudo ufw status verbose

echo "Complete Apache installation."

# Show IP to access
ip_address=$(hostname -I | awk '{print $1}')
echo "Accessing Apache at: http://$ip_address"
```

## Chạy file

```bash
chmod +x install_http.sh
sudo ./install_http.sh
```
