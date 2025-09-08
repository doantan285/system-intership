# Open vSwitch

## I. Open vSwitch là gì?

![what is open vswitch](./images/what_is_openvswitch.png)

**Open vSwitch (OVS)** là một **virtual switch** mã nguồn mở, được thiết kế để dùng trong môi trường ảo hóa và **cloud computing**. Nó hoạt động giống như một switch trong mạng vật lý, nhưng thay vì kết nối các thiết bị vật lý, OVS kết nối các máy ảo (VM), container, host trong môi trường ảo hóa.

### 1. Tổng quan

- **Open vSwitch** là một phần mềm chuyển mạch đa lớp, được cấp phép theo giấy phép mã nguồn mở Apache 2. Mục tiêu là tạo ra một nền tảng chuyển mạch chất lượng cao, hỗ trợ các giao diện quản lý tiêu chuẩn và cho phép mở rộng, kiểm soát các chức năng chuyển tiếp thông qua lập trình.
- **Open vSwitch** đặc biệt phù hợp để hoạt động như một chuyển mạch ảo trong môi trường máy ảo (VM). Ngoài việc cung cấp các giao diện kiểm soát và quan sát tiêu chuẩn cho tầng mạng ảo, nó được thiết kế để hỗ trợ phân phối trên nhiều máy chủ vật lý. Open vSwitch hỗ trợ nhiều công nghệ ảo hóa dựa trên Linux, bao gồm **KVM** và **VirtualBox**.

### 2. Các tính năng

- **Chuẩn VLAN 802.1Q** với hỗ trợ **trunk port** và **access port**.
- **NIC bonding** (gộp nhiều card mạng) có hoặc không có **LACP** trên switch vật lý upstream
- **NetFlow**, **sFlow**, **mirroring** → tăng cường khả năng giám sát và hiển thị lưu lượng
- **QoS (Quality of Service)** kèm khả năng giới hạn (policing)
- Hỗ trợ nhiều loại tunnel: **Geneve**, **GRE**, **VXLAN**, **ERSPAN**, **GTP-U**, **SRv6**, **Bareudp**.
- **802.1ag Connectivity Fault Management** (quản lý sự cố kết nối)
- **OpenFlow 1.0** kèm nhiều phần mở rộng
- **Cơ sở dữ liệu cấu hình giao dịch** với API C và Python
- **Hiệu năng cao** nhờ Linux kernel module cho forwarding

### 3. Các thành phần chính

![open vswitch components](./images/ovs-components.png)

- `ovs-vswitchd`: Daemon thực thi chuyển mạch, đi kèm với mô-đun nhân Linux để chuyển tiếp dựa trên luồng.
- `ovsdb-server`: Máy chủ cơ sở dữ liệu nhẹ, cung cấp cấu hình cho `ovs-vswitchd`.
- `ovs-dpctl`: Công cụ để cấu hình mô-đun chuyển mạch nhân.
- `Scripts và specs`: Hỗ trợ xây dựng gói RPM cho Red Hat Enterprise Linux và gói deb cho Ubuntu/Debian.
- `ovs-vsctl`: Tiện ích để truy vấn và cập nhật cấu hình của ovs-vswitchd.
- `ovs-appctl`: Tiện ích gửi lệnh đến các daemon Open vSwitch đang chạy.

### 4. Các công cụ bổ sung

- `ovs-ofctl`: Tiện ích để truy vấn và kiểm soát các chuyển mạch và bộ điều khiển OpenFlow.
- `ovs-pki`: Tiện ích để tạo và quản lý hạ tầng khóa công khai cho các chuyển mạch OpenFlow.
- `ovs-testcontroller`: Bộ điều khiển OpenFlow đơn giản, hữu ích cho việc kiểm thử (không dùng trong môi trường sản xuất).
- `Patch cho tcpdump`: Cho phép tcpdump phân tích các thông điệp OpenFlow.

## II. Các thao tác quản lý Open vSwitch
