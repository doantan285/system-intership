# Tìm hiểu các công nghệ liên quan đến KVM

## I. Linux Bridge - Công nghệ mạng ảo của KVM

### 1. Linux Bridge là gì?

Linux Bridge là một **switch layer 2 ảo** được cung cấp bởi nhân Linux. Nó cho phép:

- Nhiều thiết bị mạng (như máy ảo) giao tiếp với nhau như trong một switch vật lý.
- Có thể kết nối ra ngoài qua một card mạng thật (khi cấu hình chế độ bridged).
- Là thành phần quan trọng trong việc tạo mạng ảo với KVM/libvirt.

### 2. Cách hoạt động của Linux Bridge

- **Tạo một Bridge ảo:** Tạo một thiết bị bridge trên Host Linux, ví dụ `br0`.
- **Gắn các thiết bị vào Bridge:** gắn (attach) các thiết bị mạng khác vào bridge này. Các thiết bị đó có thể là:
  - Card mạng vật lý (pNIC) của Host (ví dụ: `eno1`).
  - Card mạng ảo (vNIC) của các máy ảo KVM.
- **Chuyển tiếp frame:** Khi một frame dữ liệu đến bridge từ một cổng nào đó (ví dụ: từ một máy ảo), bridge sẽ xem địa chỉ MAC đích của frame đó.
  - Nếu MAC đích là của một thiết bị khác cũng được gắn vào bridge (ví dụ: một máy ảo khác), bridge sẽ chuyển tiếp frame đó trực tiếp đến cổng tương ứng.
  - Nếu MAC đích là của một thiết bị nằm ngoài Host (ví dụ: một máy tính vật lý khác trên mạng LAN), bridge sẽ chuyển tiếp frame đó ra cổng vật lý (pNIC) đã được gắn vào nó.
- **Hoạt động minh bạch:** Đối với các thiết bị bên ngoài, các máy ảo trong cấu hình bridged xuất hiện như thể chúng là những máy vật lý độc lập đang cắm vào cùng một switch vật lý với Host. Chúng chia sẻ cùng một dải IP (subnet) của mạng vật lý.

### 3. Các lệnh liên quan

```bash
brctl show           # Hiển thị danh sách các bridge
ip link              # Hiển thị các card mạng
ip a                 # Hiển thị IP của bridge và các interface
```

### 4. Cấu hình bởi libvirt

- Libvirt có thể tự động tạo bridge như `virbr0` khi cấu hình NAT.
- Nếu tự tạo bridge `br0`, cần chỉnh `/etc/network/interfaces` hoặc dùng `nmcli` (nếu có NetworkManager).

## II. Công nghệ storage trong KVM

Trong KVM, storage là nơi lưu trữ đĩa ảo (VM disk) và các file liên quan đến máy ảo (ISO, snapshot,...).

### 1. Các loại lưu trữ

**File-based storage:**

- Định dạng: `.img` (raw), `.qcow2` (QEMU Copy-On-Write), `.vmdk`.
- Path: Thường nằm trong `/var/lib/libvirt/images/`.
- Ưu điểm: Dễ quản lý, hỗ trợ snapshot (đặc biệt với `.qcow2`), linh hoạt.
- Nhược điểm: Hiệu năng thấp hơn so với block device do I/O qua filesystem.

**Block device storage:**

- Ví dụ: LVM (Logical Volume Manager), iSCSI, hoặc thiết bị trực tiếp (như `/dev/sdb`).
- Ưu điểm: Hiệu năng cao, phù hợp cho máy ảo lớn hoặc tải nặng.
- Nhược điểm: Cần cấu hình phức tạp hơn.

**Network storage:**

- Ví dụ: NFS, GlusterFS, Ceph.
- Ưu điểm: Phân tán, dễ mở rộng, phù hợp cho cụm máy ảo.
- Nhược điểm: Phụ thuộc mạng, độ trễ cao hơn.

### 2. Thư mục liên quan

| Thư mục | Chức năng |
|---------|-----------|
| `/var/lib/libvirt/images/` | Nơi lưu trữ file đĩa mặc định |
| `/etc/libvirt/storage/` | Chứa XML định nghĩa storage pool |
| `/var/lib/libvirt/qemu/snapshot/` | Lưu snapshot của vm |
| `/etc/libvirt/qemu/` | Chứa XML cấu hình máy ảo (VM definition) |

### 3. Sotrage Pool và Volume

KVM sử dụng khái niệm:

- **Storage Pool:** một tập hợp dung lượng lưu trữ, ví dụ `/var/lib/libvirt/images`.
- **Sotrage Volume:** một file đĩa ảo cụ thể (như `ubuntu.qcow2`).

Có thể quản lý bằng lệnh:

```bash
virsh pool-list
virsh vol-list default
virsh pool-define /path/to/pool.xml
```

### 4. Snapshot

- Nếu dùng `.qcow2`, có thể tạo snapshot bằng `vỉrsh snapshot-create`.
- Snapshot giúp lưu trạng thái máy ảo (đĩa + RAM nếu muốn)
