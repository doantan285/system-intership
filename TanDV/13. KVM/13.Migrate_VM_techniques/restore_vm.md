# Restore máy ảo trong trường hợp máy ảo bị chết

Để khôi phục một máy ảo (VM) trong KVM khi nó bị chết (crash, không khởi động được, hoặc dữ liệu bị hỏng), có thể thực hiện các bước sau tùy thuộc vào tình huống cụ thể (có snapshot, backup, hoặc chỉ có file đĩa).

## I. Xác định nguyên nhân và tình trạng

### 1. Kiểm tra log

```bash
journalctl -u libvirtd
```

- Xác định lỗi (ví dụ: lỗi đĩa, bộ nhớ, hoặc cấu hình).

### 2. Kiểm tra trạng thái VM

```bash
virsh list --all
```

- Nếu VM không xuất hiện hoặc ở trạng thái "shut off" bất thường, cần khôi phục.

### 3. Kiểm tra file đĩa

```bash
ls -lh /var/lib/libvirt/images/<vm-disk>.qcow2
```

- Đảm bảo file đĩa vẫn tồn tại và không bị hỏng.

## II. Phương pháp khôi phục

### 1. Khôi phục từ snapshot (nếu có)

**Điều kiện:** VM đã được cấu hình snapshot trước đó.

**Quy trình:**

1. Liệt kê các snapshot:

   ```bash
   virsh snapshot-list <vm-name>
   ```

2. Khôi phục từ snapshot:

   ```bash
   virsh snapshot-revert <vm-name> <snapshot-name>
   ```

3. Khởi động lại VM:

   ```bash
    virsh start <vm-name>
    ```

> Lưu ý: Snapshot lưu trạng thái tại một thời điểm, không bao gồm dữ liệu mới sau snapshot.

### 2. Khôi phục từ backup (nếu có)

**Điều kiện:** Đã sao lưu file đĩa (.qcow2) và file XML cấu hình trước đó.

**Quy trình:**

1. Khôi phục file đĩa:

   ```bash
   cp /backup/location/<vm-disk>.qcow2 /var/lib/libvirt/images/
   ```

2. Khôi phục file XML:

    ```bash
    cp /backup/location/<vm-name>.xml /etc/libvirt/qemu/
    ```

3. Định nghĩa lại VM:

   ```bash
   virsh define /etc/libvirt/qemu/<vm-name>.xml
   ```

4. Khởi động lại VM:

   ```bash
    virsh start <vm-name>
    ```

> Lưu ý: Đảm bảo quyền file (chown qemu:qemu nếu cần).

### 3. Khôi phục từ file đĩa hiện có (không có snapshot/backup)

**Điều kiện:** File đĩa vẫn còn nguyên vẹn, chỉ cấu hình hoặc trạng thái bị lỗi.

**Quy trình:**

1. Xóa định nghĩa VM cũ (nếu còn):

   ```bash
   virsh undefine <vm-name>
   ```

2. Tạo file XML thủ công hoặc từ bản sao cũ:

   - Ví dụ nội dung XML cơ bản:

    ```xml
    <domain type='kvm'>
    <name><vm-name></name>
    <memory unit='MiB'>2048</memory>
    <vcpu>1</vcpu>
    <os>
        <type arch='x86_64'>hvm</type>
        <boot dev='hd'/>
    </os>
    <devices>
        <disk type='file' device='disk'>
        <driver name='qemu' type='qcow2'/>
        <source file='/var/lib/libvirt/images/<vm-disk>.qcow2'/>
        <target dev='vda' bus='virtio'/>
        </disk>
        <interface type='network'>
        <source network='default'/>
        </interface>
        <graphics type='vnc' port='-1'/>
    </devices>
    </domain>
    ```

   - Lưu vào `/etc/libvirt/qemu/<vm-name>.xml`.

3. Định nghĩa lại VM:

   ```bash
    virsh define /etc/libvirt/qemu/<vm-name>.xml
    ```

4. Khởi động VM:

   ```bash
    virsh start <vm-name>
    ```

> Lưu ý: Nếu file đĩa hỏng, dùng `qemu-img check <vm-disk>.qcow2` để kiểm tra và sửa (nếu có thể).

### 4. Tái tạo VM từ ISO (nếu không thể khôi phục)

**Điều kiện:** Dữ liệu không quan trọng hoặc không thể khôi phục file đĩa.

**Quy trình:**

1. Xóa VM cũ:

   ```bash
   virsh undefine <vm-name>
   rm /var/lib/libvirt/images/<vm-disk>.qcow2
   ```

2. Tạo lại VM mới với `virt-install`:

   ```bash
   sudo virt-install \
     --name <vm-name> \
     --ram 2048 \
     --vcpus 1 \
     --disk path=/var/lib/libvirt/images/<vm-disk>.qcow2,size=10,format=qcow2 \
     --cdrom /var/lib/libvirt/iso/CentOS-Stream-9-20250728.1-x86_64-boot.iso \
     --os-variant rhel9.0 \
     --network network=default \
     --graphics vnc,listen=0.0.0.0 \
     --noautoconsole
   ```

3. Kết nối VNC để cài đặt hệ điều hành.

## III. Kiểm tra sau khôi phục

### 1. Kiểm tra trạng thái VM

```bash
virsh list --all
```

### 2. Kiểm tra log

```bash
virsh log <vm-name>
```

### 3. Kiểm tra kết nối mạng

- Nếu VM có IP, thử ping hoặc SSH.

## IV. Lưu ý quan trọng

- **Backup định kỳ:** Sao lưu file đĩa và XML trước khi xảy ra sự cố.
- **Snapshot:** Sử dụng virsh snapshot-create-as để tạo điểm khôi phục.
- **Phân quyền:** Đảm bảo file đĩa có quyền qemu:qemu (chown nếu cần).
- **ung lượng:** Kiểm tra không gian lưu trữ trước khi khôi phục.

## V. Ví dụ

- VM tên `my-vm`, file đĩa `/var/lib/libvirt/images/my-vm.qcow2` bị lỗi.
- Khôi phục từ backup:

    ```bash
    cp /backup/my-vm.qcow2 /var/lib/libvirt/images/
    cp /backup/my-vm.xml /etc/libvirt/qemu/
    virsh define /etc/libvirt/qemu/my-vm.xml
    virsh start my-vm
    ```
