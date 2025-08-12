# Tìm hiểu Virsh Client

**virsh client** thường được gọi ngắn gọn là **virsh**

## I. Virsh là gì?

`virsh` là một **command-line interface (CLI)** được sử dụng để quản lý các máy ảo (VMs) và các thành phần ảo hóa khác. Nó là giao diện chính để tương tác với `libvirtd` daemon, một dịch vụ chạy nền trên Host Linux chịu trách nhiệm quản lý KVM và các hypervisor khác (như QEMU, Xen).

`virsh` có thể được sử dụng:

- **Tương tác với libvirt:** Gửi các lệnh đến libvirtd để thực hiện các tác vụ như khởi động, tắt, tạo, xóa máy ảo, v.v.
- **Theo dõi trạng thái:** Hiển thị thông tin về các máy ảo, mạng ảo, và lưu trữ.
- **Thực hiện các tác vụ quản trị:** Chỉnh sửa cấu hình, tạo snapshot, di chuyển máy ảo.

## II. cú pháp cơ bản

```bash
virsh [options] <command> [args...]
```

**Ví dụ:**

```bash
virsh list --all
virsh start vm-name
virsh shutdown vm-name
```

## III. Các lệnh cơ bản

### 1. Quản lý máy ảo (domain)

- `virsh list`: Liệt kê máy ảo đang chạy.
- `virsh list --all`: Liệt kê toàn bộ máy ảo (cả đang tắt).
- `virsh start <vm>`: Bật máy ảo.
- `virsh shutdown <vm>`: Tắt máy ảo một cách an toàn.
- `virsh destroy <vm>`: Tắt máy ảo ngay lập tức (giống như tắt nguồn).
- `virsh reboot <vm>`: Khởi động lại máy ảo.
- `virsh suspend <vm>`: Tạm dừng máy ảo.
- `virsh resume <vm>`: Tiếp tục máy ảo đã tạm dừng.
- `virsh undefined <vm>`: Xóa thông tin cấu hình (XML), không xóa đĩa.
- `virsh edit <vm>`: Mở file XML cấu hình VM bằng trình soạn thảo.
- `virsh dumpxml <vm>`: Hiển thị cấu hình XML của máy ảo.

### 2. Quản lý snapshot

- `virsh snapshot-create <vm>`: Tạo snapshot
- `virsh snapshot-list <vm>`: Liệt kê các snapshot của máy ảo.
- `virsh snapshot-revert <vm> --snapshotname <name>`: Quay lại snapshot đã tạo.

### 3. Quản lý storage

- `virsh vol-list <pool>`: Liệt kê các volume trong storage pools.
- `virsh vol-list default`: Liệt kê các volume trong storage pool mặc định.
- `virsh vol-define <xml-file>`: Định nghĩa một volume mới từ file XML.
- `virsh vol-create-as default ubuntu.qcow2 10G --format qcow2`: Tạo một volume mới có tên `ubuntu.qcow2` với kích thước 10GB trong storage pool mặc định.

### 4. Quản lý mạng (network)

- `virsh net-list --all`: Liệt kê mạng ảo
- `virsh net-start <network>`: Bật mạng ảo.
- `virsh net-destroy <network>`: Tắt mạng ảo.
- `virsh net-define <file.xml>`: Định nghĩa một mạng ảo mới từ file XML.
- `virsh net-autostart <network>`: Thiết lập mạng ảo tự động khởi động khi hệ thống khởi động.

### 5. Một số lệnh hữu ích khác

- `virsh console <vm>`: Mở console vm (nếu cấu hình được).
- `virsh dominfo <vm>`: Xem thông tin tổng quan máy ảo.
- `virsh domiflist <vm>`: Xem thông tin NIC của máy ảo.
- `virsh domblklist <vm>`: Xem thông tin các đĩa của máy ảo.

## IV. Tập tin cấu hình liên quan đến virsh

| Thư mục | Mô tả |
| --- | --- |
| `/etc/libvirt/qemu/` | Chứa file cấu hình XML máy ảo |
| `/etc/libvirt/qemu/networks/` | Cấu hình mạng ảo |
| `/etc/libvirt/storage/` | Cấu hình storage pool |
| `/var/lib/libvirt/images/` | Mặc định lưu đĩa `.qcow2` |
