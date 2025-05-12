# Script tự động tải Wordpress

## Mục tiêu

1. Cài **Apache**, **MySQL**, **PHP**.
2. Tải và giải nén WordPress.
3. Tạo cơ sở dữ liệu WordPress.
4. Cấu hình thư mục Apache `/var/www/html/wordpress`.

## Nội dung file `install_wordpress.sh`

```bash
#!/bin/bash

# Check not root or not using sudo
if [[ $EUID -ne 0 ]] then
    echo "Please run the script with sudo or root privileges"
fi

echo "updating system..."
sudo apt update -y && apt upgrade -y

echo "installing Apache HTTP Server..."
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2

echo "Installing PHP and extentions"
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y

echo "Installing MySQL and create database..."
sudo apt install mysql-server -y

mysql_secure_installation <<EOF
y
n
y
y
y
EOF

echo "Creating database and user..."
mysql -u root <<EOF
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'doantan28';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EOF

# Install wordpress
echo "downloading wordpress..."
cd /tmp
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzvf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/

# Modify Apache configuration to support .htaccess...
sed -i '/<VirtualHost \*:80>/,/<\/VirtualHost>/s|DocumentRoot /var/www/html|DocumentRoot /var/www/html\n\t<Directory /var/www/html>\n\t\tAllowOverride All\n\t</Directory>|' /etc/apache2/sites-available/000-default.conf

sudo a2enmod rewrite
sudo systemctl restart apache2

echo "Complete Wordpress installation!"
```

## Chạy file

```bash
chmod +x install_wordpress.sh
sudo install_wordpress.sh
```

Mở trình duyệt và truy cập:

```htpp
http://<IP_address_or_domain_name>
```
