# IPv6

## Khái niệm

IPv6 (Internet Protocol version 6) là phiên bản mới nhất của giao thức Internet (IP), được thiết kế để thay thế IPv4 do tình trạng thiếu địa chỉ IP trên IPv4. Với không gian địa chỉ rộng lớn lên đến 128 bit, IPv6 mang đến khả năng kết nối cho hàng tỷ thiết bị trên toàn cầu. IPv6 có không gian địa chỉ 128 bit, cung cấp lượng địa chỉ khổng lồ.

IPv6 sẽ có dạng như sau: 2001:0db8:85a3:0000:0000:8a2e:0370:7334

## Cấu trúc

![structure](../images/structure.png)

**Địa chỉ IPv6** dài 128 bit, được chia làm 8 nhóm, mỗi nhóm gồm 16 bit, được ngăn cách với nhau bằng dấu hai chấm “:”. Mỗi nhóm được biểu diễn bằng 4 số hexa.

VD:

- `FEDC:BA98:768A:0C98:FEBA:CB87:7678:1111`
- `1080:0000:0000:0070:0000:0989:CB45:345F`

### Nguyên tắc nhằm rút gọn địa chỉ

- Cho phép bỏ các số 0 nằm trước mỗi nhóm (octet).
- Thay bằng số 0 cho nhóm có toàn số 0.
- Thay bằng dấu “::” cho các nhóm liên tiếp nhau có toàn số 0.

Ví dụ: Địa chỉ `1080:0000:0000:0070:0000:0989:CB45:345F` được nén thành `1080::70:0:989:CB45:345F`

*Chú ý: Dấu “::” chỉ sử dụng được 1 lần trong toàn bộ địa chỉ IPv6 (nhiều dấu “::” có thể gây ra sự nhầm lẫn hoặc không thể biết đúng vị trí của các octet trong địa chỉ IPv6).*

## Thành phần

![components](../images/components.jpg)

Địa chỉ IPv6 gồm 3 phần chính: Site Prefix, Subnet ID, InterfaceID. Các thành phần này tạo thành địa chỉ IPv6 hoàn chỉnh, mang đến không gian địa chỉ mở rộng, đảm bảo tính riêng tư và bảo mật, cũng như cung cấp sự linh hoạt để mạng có thể phát triển và đáp ứng nhu cầu kết nối internet ngày càng tăng.

### Site Prefix

- Đây là số ISP (Nhà cung cấp Dịch vụ Internet), được gán cho mỗi trang web. Site Prefix thường áp dụng cho tất cả máy tính ở cùng một vị trí. Bằng cách chia sẻ một Site Prefix, các máy tính trong mạng tự động nhận biết và cho phép mạng truy cập internet.
- Trong ảnh trên:
  - Site Prefix: `2001:0db8:3c4d`
  - Vai trò: Định danh một tổ chức hoặc một nhà cung cấp dịch vụ.

### Subnet ID

- Phần này miêu tả cấu trúc của mạng con trong trang web. Subnet ID giúp tổ chức chia mạng thành các đơn vị nhỏ hơn, gọi là subnet. Mỗi IPv6 subnet có cấu trúc tương tự như mạng con IPv4.
- Trong ảnh trên:
  - Subnet ID: `0015`
  - Vai trò: Giúp tổ chức hoặc doanh nghiệp chia nhỏ mạng thành các phân đoạn dễ quản lý hơn.

### Interface ID

- Là phần định danh duy nhất của một thiết bị trong mạng. Interface ID giúp xác định một thiết bị cụ thể trong mạng. Định dạng EUI-64 thường được sử dụng để định dạng giao diện của Interface ID.
- Trong ảnh trên:
  - Interface ID: `0000:0000:1a2f:1a2b`
  - Vai trò: Mỗi thiết bị trong mạng có một Interface ID duy nhất để giao tiếp.

## Phân loại địa chỉ IPv6

### IPv6 Unicast

Địa chỉ Unicast đại diện cho một thiết bị duy nhất trong mạng. Dữ liệu được gửi đến một địa chỉ Unicast sẽ chỉ đến đúng thiết bị đó. IPv6 Unicast dùng để định danh các thiết bị (như máy tính, máy chủ) trong mạng, tương tự như địa chỉ IP trong IPv4. IPv6 unicast gồm:

***Global Unicast Address (GUA):***

- Global unicast addresses (GUA) là địa chỉ IPv6 toàn cầu (tương tự như địa chỉ public của IPv4). Phạm vi định vị của GUA là toàn hệ thống IPv6 trên thế giới.

![gua](../images/gua.png)

- 001: Ba bit đầu luôn có giá trị là 001 (Prefix=2000::/3).
- Global Routing Prefix (45 bit): Là địa chỉ được cung cấp cho công ty, cơ quan, tập đoàn hay một tổ chức nào đó khi đăng ký địa chỉ IPv6 public.
- Subnet ID (16 bit): Các tổ chức có thể tự quản lý, phân bổ, cấp phát và tổ chức định tuyến bên trong mạng của mình. Với 16 bit, có thể tạo nên 65535 mạng con (subnet) hoặc nhiều cấp định tuyến phân cấp hiệu quả trong mạng của tổ chức.
- Interface ID (64 bit): là địa chỉ của các interface trong subnet.

***Link-local Address (LLA):***

- Sử dụng bởi các node khi giao tiếp với các node lân cận (neighbor node). Phạm vi của dạng địa chỉ unicast này là trên một đường kết nối (phạm vi link). Địa chỉ link-local luôn được cấu hình một cách tự động.

![lla](../images/lla.png)

- Địa chỉ link-local bắt đầu bởi 10 bit prefix là FE80::/10, tiếp theo là 54 bit 0, 64 bit còn lại là phần định danh giao diện (interface ID).
- VD:
  - `FE80::1CEF:01BC:FE01:1101`
  - `FE80::2:1991:AC90:CF10`

***Unique Local Address (ULA):***

- là một loại địa chỉ IPv6 được thiết kế để sử dụng trong mạng nội bộ (LAN hoặc tổ chức), tương tự như địa chỉ Private IP trong IPv4.

![ula](../images/ula.png)

- Prefix: Luôn có giá trị FC (1111 110) hoặc FD (1111 1101).
- L (Local bit): 1: ULA (FD00::/8), 0: Reserved (FC00::/8).
- Global ID: Mã nhận dạng duy nhất, được tạo ngẫu nhiên (tránh trùng lặp).
- Ví dụ:
  - `FD12:3456:789a::1`
  - `FD00:1234:5678:abcd::1`

### IPv6 Multicast

Multicast cho phép gửi dữ liệu đến nhiều thiết bị trong cùng một nhóm. Dữ liệu sẽ được nhận bởi tất cả các thiết bị thuộc nhóm Multicast. IPv6 Multicast được dùng để phát trực tiếp nội dung (ví dụ: hội nghị truyền hình, phát sóng video), quản lý thiết bị trong mạng (ví dụ: thông báo định tuyến).

![multicast](../images/multicast.png)

- 8 bit đầu tiên trong địa chỉ multicast luôn luôn là 1111 1111 (FF - hexadecimal).
- Flag (4 bits): Điều khiển đặc tính của multicast (ví dụ: 0 – tĩnh, 1 – tạm thời).
- Scope (4 bits): Xác định phạm vi multicast (xem bảng bên dưới).
![scope](../images/scope.png)
- Group ID được sử dụng để xác định một nhóm multicast. Có những group ID được định nghĩa từ trước (predefined group ID), như Group ID =1 tức là mọi node.
- VD: Địa chỉ multicast là `ff02::1` có nghĩa Scope ID = 2 và Group ID =1, chỉ định mọi node trong phạm vi một link (link-scope). Điều này giống như broadcast trong địa chỉ IPv4

### IPv6 Anycast

Địa chỉ Anycast cho phép gửi dữ liệu đến thiết bị gần nhất (theo khoảng cách định tuyến) trong một nhóm thiết bị có cùng địa chỉ. IPv6 Anycast được dùng để tối ưu hóa lưu lượng mạng và giảm độ trễ, thường được sử dụng trong các trung tâm dữ liệu và máy chủ DNS.

**Đặc điểm:**

- Các thiết bị chia sẻ cùng một địa chỉ Anycast.
- Dữ liệu sẽ được chuyển đến thiết bị có đường dẫn ngắn nhất hoặc hiệu quả nhất.

## IPv6/IPv4 header

![header](../images/header.png)

### Các trường giữ nguyên

- **Version:** Xác định phiên bản IP (4 đối với IPv4 và 6 đối với IPv6).
- **Source Address:** địa chỉ thiết bị gửi.
- **Destination Address:** địa chỉ thiết bị nhận.

### Các trường được giữ nhưng đổi tên

- **Traffic class (IPv6) vs Type of Service (IPv4):**
  - Xác định mức độ ưu tiên và chất lượng dịch vụ (QoS).
  - Có 2 phần: DSCP và ECN.
  - DSCP (Differentiated Services Code Point - 6 bit): Đánh dấu mức độ ưu tiên.
  - ECN (Explicit Congestion Notification - 2 bit): Báo hiệu tắc nghẽn.
- **Payload Length (IPv6) vs Total Length (IPv4):**
  - Payload Length: Chỉ xác định kích thước phần dữ liệu (payload), không tính header chính.
  - Total Length: Xác định tổng kích thước của toàn bộ gói tin (header + payload).
  - Tối đa 65.535 byte.
- **Next Header (IPv6) vs Protocol (IPv4):**
  - Next Header:Xác định phần kế tiếp (giao thức hoặc extension header).
  - Protocol: Xác định giao thức lớp trên: TCP (6), UDP (17), ICMP (1).
  - IPv4: Chỉ cho biết giao thức lớp trên. VD: TCP = 6, UDP = 17, ICMP = 1.
  - IPv6: Linh hoạt hơn, hỗ trợ cả giao thức và các header mở rộng. VD: TCP = 6, UDP = 17, Fragment Header = 44, ESP = 50.
- **Hop Limit (IPv6) vs TTL (IPv4)**
  - Giới hạn số lần truyền qua router (hops), tránh vòng lặp vô thời hạn.
  - Mỗi khi qua một router, giá trị này giảm đi 1. Nếu giảm xuống 0, gói tin bị loại bỏ.

### Các trường còn lại

- **Flow Label (IPv6)** cải thiện QoS và tăng tốc xử lý gói tin.

- Các trường không còn dùng trong IPv6:
  - IHL (Internet Header Length): IPv6 có kích thước header cố định là 40 byte, không thay đổi, nên không cần trường này.
  - Identification, Flags, Fragment Offset: IPv6 không cho phép router trung gian phân mảnh gói tin, chỉ có thiết bị gửi (source) thực hiện. Điều này giảm độ trễ và tăng tốc độ xử lý tại router.
  - Header Checksum: IPv6 loại bỏ kiểm tra lỗi ở header vì tầng giao vận (TCP/UDP) và tầng liên kết dữ liệu đã kiểm tra lỗi rồi. Điều này giúp giảm tải cho router, tăng tốc độ xử lý.
  - Options và Padding: Các chức năng tùy chọn được chuyển sang Extension Header, giúp header chính của IPv6 đơn giản hơn và dễ mở rộng khi cần. Kích thước IPv6 là cố định nên padding là không cần thiết.

### Điểm khác nhau

| Đặc điểm | IPv6 | IPv4 |
|-----------|-------------|---------|
| Kích thước header | Cố định 40 byte. | Biến đổi (20 – 60 byte). |
| Độ dài địa chỉ | 128 bit (16 byte) | 32 bit (4 byte) |
| Checksum | Không (giảm tải xử lý tại router). | Có (kiểm tra lỗi ở header). |
| Phân mảnh | Chỉ thực hiện tại thiết bị gửi. | Được thực hiện tại thiết bị gửi và router. |
| Next Header | Chỉ định header tiếp theo hoặc extension. | Protocol (giao thức lớp trên). |
| QoS | Traffic Class + Flow Label (28 bit). | Type of Service (TOS – 8 bit). |
| Option | Dùng extension header bên ngoài. | Nằm trong header chính. |
| Bảo mật | Hỗ trợ IPSec mặc định. | Không tích hợp sẵn. |
| Tối ưu hóa tốc độ | Nhanh hơn nhờ header đơn giản hơn. | Chậm hơn do nhiều trường kiểm tra. |

## Dual stack

![dual-stack](../images/dual-stack.png)

Dual-stack (hay còn gọi là "xếp chồng kép") là một cơ chế cho phép cả hai giao thức IPv4 và IPv6 cùng hoạt động trên một thiết bị hoặc mạng. Điều này có nghĩa là thiết bị có thể giao tiếp với các thiết bị khác bằng cả hai giao thức.

- Ứng dụng hỗ trợ cả IPv4 & IPv6, có thể sử dụng cả hai giao thức.
- Giao thức vận chuyển: TCP/UDP hoạt động với cả hai giao thức mạng.
- Lớp mạng có cả IPv4 & IPv6, ứng dụng sẽ chọn sử dụng IPv4 hoặc IPv6 tùy thuộc vào loại kết nối.
- Ethernet có thể sử dụng cả hai Protocol ID:
  - `0x0800` cho IPv4.
  - `0x86DD` cho IPv6.

### Cách thức hoạt động

- Hai giao thức cùng tồn tại: Dual-stack cho phép cả IPv4 và IPv6 cùng tồn tại trên một thiết bị hoặc mạng.
- Lựa chọn giao thức: Khi một thiết bị muốn giao tiếp với một thiết bị khác, nó sẽ lựa chọn giao thức nào phù hợp nhất. Nếu cả hai thiết bị đều hỗ trợ cả IPv4 và IPv6, thiết bị gửi thường ưu tiên sử dụng IPv6 vì nó có nhiều ưu điểm hơn.
- Tương thích ngược: Dual-stack cho phép các thiết bị IPv6 giao tiếp với các thiết bị IPv4 thông qua các cơ chế chuyển đổi như NAT64 hoặc đường hầm (tunneling).

### Ưu/Nhược điểm của Dual-stack

`Ưu điểm`

- Tương thích: Dual-stack cho phép các thiết bị IPv6 giao tiếp với các thiết bị IPv4, đảm bảo tính tương thích trong quá trình chuyển đổi từ IPv4 sang IPv6.
- Linh hoạt: Dual-stack cho phép các thiết bị lựa chọn giao thức phù hợp nhất cho từng tình huống, tận dụng ưu điểm của cả hai giao thức.
- Dễ dàng triển khai: Dual-stack là một cơ chế chuyển đổi tương đối dễ dàng triển khai, không yêu cầu thay đổi lớn trong cơ sở hạ tầng mạng.

`Nhược điểm`

- Phức tạp: Dual-stack yêu cầu cấu hình và quản lý cả hai giao thức IPv4 và IPv6, có thể làm phức tạp hệ thống mạng.
- Tốn kém tài nguyên: Dual-stack yêu cầu thiết bị phải có đủ tài nguyên (bộ nhớ, CPU) để xử lý cả hai giao thức, có thể làm tăng chi phí đầu tư.
