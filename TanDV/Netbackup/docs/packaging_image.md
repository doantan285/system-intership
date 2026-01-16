# Đóng gói image

## I. Trên openstack

### 1. Chuẩn bị trước khi đóng gói

```bash
# Dừng Netbackup services
/usr/openv/netbackup/bin/goodies/netbackup stop
# Kiểm tra
/usr/openv/netbackup/bin/goodies/netbackup status
```

Gỡ thông tin “machine-specific” (bắt buộc):

```bash
# xóa host identity
rm -f /usr/openv/netbackup/db/host_id
# Xóa cache web
rm -rf /usr/openv/netbackup/logs/*
rm -rf /usr/openv/tmp/*
```

Reset hostname (rất quan trọng với NetBackup), Netbackup cực kỳ nhạy hostname:

```bash
# Chỉ giữ hostname chung chung
hostnamectl set-hostname netbackup-template
```

(Khuyến nghị) Tắt NetBackup auto-start

```bash
systemctl disable netbackup
```

shutdown VM:

```bash
shutdown -h now
```

### 2. Tạo image trên OpenStack

#### Bước 1: Xác định root volume

Vào **Project** → **Volumes** → **Volumes**

- Tìm volume có:
  - Attached to: VM Netbackup
  - Bootable: Yes

#### Bước 2: Detach volume (nếu cần)

Nếu OpenStack không cho create image khi volume đang attach: Stop instance trước (đã shutdown rồi).

#### Bước 3: Create Image từ volume

Trong horizon: **Volumes** → **Volumes** → **Action** → **Create Image**

Điền:

- Image name: netbackup11-golden
- Disk format: qcow2
- Visibility: Private (hoặc Shared)

-> Create

### 3. Dùng image này để tạo VM NetBackup mới

Khi tạo instance:

```makefile
Source: Image
Image: netbackup11-golden
```

- BẮT BUỘC làm sau khi boot VM mới.

### 4. Việc PHẢI làm sau khi tạo VM mới từ image

Set hostname mới:

```bash
hostnamectl set-hostname <new-hostname>
# Ví dụ
hostnamectl set-hostname netbackup01.lab.local
```

Update /etc/hosts:

```bash
vim /etc/hosts
# 192.168.70.15  netbackup01.lab.local netbackup01
```

Regenerate NetBackup host identity:

```bash
/usr/openv/netbackup/bin/admincmd/nbhostidentity -init
```

Enable & start NetBackup:

```bash
systemctl enable netbackup
systemctl start netbackup
```

Reset admin password Web UI:

```bash
/usr/openv/netbackup/bin/admincmd/nbwmc reset-admin-password
# Nhập password mới
```

### 5. Kiểm tra web UI

Truy cập: `https://<hostname>:1556`

Nếu:

- Web UI lên
- Login được admin
- Không lỗi cert

=> Đóng gói thành công.

## II. Trên VMware Workstation


