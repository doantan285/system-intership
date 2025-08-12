# Tìm hiểu kỹ thuật Migrate VM

## I. Kỹ thuật Migrate là gì?

Kỹ thuật **migrate** trong **KVM (Kernel-based Virtual Machine)** cho phép di chuyển máy ảo (VM) từ một host này sang host khác mà không làm gián đoạn (**live migrate**) hoặc với gián đoạn tối thiểu (**cold migrate**).

## II. Các loại Migrate trong VM

| Loại migrate | Có downtime không? | Mô tả ngắn |
|--------------|--------------------|------------|
| **Live migrate** | Không | Di chuyển VM đang chạy, bộ nhớ RAM và trạng thái CPU được đồng bộ hóa giữa hai host. |
| **Cold migrate** | Có | Dừng VM → chuyển file cấu hình, disk → khởi động lại trên host mới. |

### 1. Live migrate

**Định nghĩa:** Di chuyển máy ảo đang chạy mà không làm gián đoạn dịch vụ, nhờ đồng bộ bộ nhớ và CPU trong thời gian thực.

**Cách hoạt động:**

- **KVM** sử dụng **QEMU** để theo dõi và truyền dữ liệu bộ nhớ từ host nguồn sang host đích.
- **Quá trình bao gồm các giai đoạn:** pre-copy (sao chép ban đầu), iteration (đồng bộ các trang bộ nhớ thay đổi), và stop-and-copy (chuyển trạng thái cuối cùng khi VM tạm dừng rất ngắn).

**Ưu điểm:**

- Không gián đoạn (downtime chỉ mili giây).
- Phù hợp cho môi trường HA (High Availability).

**Nhược điểm:**

- Yêu cầu cấu hình phức tạp (shared storage, mạng nhanh, CPU tương thích).

**Ứng dụng:** Bảo trì phần cứng, cân bằng tải trong cụm.

### 2. Cold migrate

**Định nghĩa:** Di chuyển máy ảo khi nó đang tắt (shutdown) hoặc tạm dừng (suspended).

**Cách hoạt động:** File đĩa (ví dụ: `.qcow2`, `.img`) và cấu hình (file XML) được sao chép từ host nguồn sang host đích. Không có đồng bộ bộ nhớ vì VM không chạy.

**Ưu điểm:**

- Đơn giản, không yêu cầu shared storage.
- Phù hợp cho di chuyển định kỳ hoặc thử nghiệm.

**Nhược điểm:**

- Gây gián đoạn dịch vụ vì VM phải tắt.

**Ứng dụng:** Nâng cấp phần cứng, di chuyển giữa các host không tương thích.

## III. Yêu cầu kỹ thuật

### 1. Yêu cầu chung (2 loại)

- **Libvirt:** Phiên bản mới nhất (hỗ trợ migrate, ví dụ: 4.0 trở lên).
- **QEMU/KVM:** Phiên bản tương thích giữa host nguồn và đích.
- **Mạng:** Kết nối ổn định giữa hai host (thường qua SSH hoặc TCP).

### 2. Riêng cho Live migrate

- **Shared Storage:** Cả hai host phải truy cập chung storage (NFS, iSCSI, Ceph, GlusterFS) để file đĩa không cần sao chép.
- **CPU Compatibility:** Cả hai host phải có CPU cùng kiến trúc (ví dụ: Intel hoặc AMD) và hỗ trợ cùng tập lệnh (xác minh bằng `virsh cpu-compare`).
- **Mạng nhanh:** Băng thông cao, độ trễ thấp để đồng bộ bộ nhớ.
- **Firewall:** Mở cổng 16509 (mặc định cho libvirt migration) hoặc tùy chỉnh.
