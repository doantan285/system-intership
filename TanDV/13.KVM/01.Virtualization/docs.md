# Tìm hiểu về ảo hóa

## I. Ảo hóa?

### 1. Ảo hóa là gì?

![What is Virtualization?](./images/virtualization.png)

**Ảo hóa** là công nghệ tạo phiên bản ảo của tài nguyên phần cứng (máy chủ, lưu trữ, mạng) để chạy nhiều máy ảo trên một máy vật lý. Phần mềm ảo mô phỏng chức năng phần cứng, cho phép chia sẻ tài nguyên (bộ nhớ, CPU, lưu trữ) giữa các môi trường ảo độc lập.

**Lợi ích:**

- Chạy 1 hệ điều hành khác ngay trên hệ thống hiện tại mà không cần phải đổi máy hay đổi hệ thống.
- Tối ưu tài nguyên, tăng hiệu suất phần cứng, giảm chi phí.
- Chuyển đổi linh hoạt giữa các hệ điều hành mà không cần khởi động lại.
- Hỗ trợ điện toán đám mây, quản lý cơ sở hạ tầng hiệu quả.

### 2. Mục đích sử dụng

- **Tối ưu tài nguyên:** Sử dụng hiệu quả phần cứng, giảm số lượng máy vật lý, tiết kiệm chi phí điện, không gian, làm mát.
- **Tăng linh hoạt:** Chạy nhiều hệ điều hành/máy ảo trên một máy, dễ dàng chuyển đổi, triển khai ứng dụng.
- **Quản lý đơn giản:** Tự động hóa quản lý CNTT, cấu hình nhất quán, giảm lỗi thủ công.
- **Phục hồi thảm họa:** Khôi phục nhanh môi trường ảo sau sự cố, đảm bảo hoạt động liên tục.
- **Hỗ trợ đám mây:** Cung cấp nền tảng cho điện toán đám mây, tăng khả năng mở rộng và quản lý cơ sở hạ tầng.
- **Kiểm thử và phát triển:** Tạo môi trường ảo để thử nghiệm phần mềm, hệ điều hành mà không ảnh hưởng hệ thống chính.
- **Bảo mật:** Cô lập môi trường ảo, giảm rủi ro lây lan phần mềm độc hại, tăng kiểm soát truy cập.

### 3. Cách thức hoạt động

1. **Lớp ảo hóa (Hypervisor):** Phần mềm hoặc phần cứng (hypervisor) tạo và quản lý máy ảo. Hypervisor tách biệt tài nguyên phần cứng vật lý (CPU, RAM, lưu trữ, mạng) và phân bổ chúng cho các máy ảo.
2. **Máy ảo (VM):** Mỗi máy ảo là môi trường độc lập, mô phỏng phần cứng ảo (CPU, RAM, ổ cứng, card mạng), chạy hệ điều hành và ứng dụng riêng.
3. **Phân bổ tài nguyên:** Hypervisor chia sẻ tài nguyên phần cứng giữa các máy ảo, đảm bảo chúng hoạt động độc lập, không xung đột.
4. **Mô phỏng phần cứng:** Phần mềm ảo hóa mô phỏng chức năng phần cứng, cho phép máy ảo hoạt động như máy vật lý thực thụ.
5. **Quản lý và giao tiếp:** Hypervisor giám sát, quản lý hiệu suất, và cung cấp giao diện để cấu hình, tạo, xóa máy ảo. Máy ảo giao tiếp với phần cứng qua hypervisor.

## II. Hypervisor

![Hypervisor](./images/hypervisor.png)

### 1. Hypervisor là gì?

**Hypervisor - trình quản lý máy ảo (Virtual Machine Monitor - VMM)**, là phần mềm hoặc phần cứng trung gian, giúp tạo và quản lý nhiều máy ảo (VM) trên một máy chủ vật lý.

**Hypervisor** cho phép chia sẻ tài nguyên vật lý (CPU, RAM, Disk, NIC...) cho nhiều máy ảo mà vẫn đảm bảo sự tách biệt, an toàn và hiệu quả.

**Các chức năng chính của Hypervisor:**

| Chức năng | Mô tả |
|-----------|-------|
| **Tạo máy ảo (VM)** | Tạo vCPU, vRAM, ổ cứng ảo, NIC ảo cho mỗi máy ảo |
| **Quản lý tài nguyên** | Phân phối CPU, RAM, Disk… giữa các VM |
| **Cô lập và bảo vệ VM** | Đảm bảo VM không ảnh hưởng nhau |
| **Giao tiếp phần cứng** | Là trung gian giữa VM và phần cứng |
| **Tính năng nâng cao** | Hỗ trợ snapshot, rollback, live migration, HA, scaling |

### 2. Phân loại Hypervisor

**Hypervisor** được chia thành 2 loại chính:

![Hypervisor Types](./images/hypervisor_types.png)

**Hypervisor loại 1 (Bare-metal Hypervisor):**

- Được cài đặt trực tiếp trên phần cứng vật lý của máy chủ, không cần hệ điều hành máy chủ (host OS) bên dưới. Nó truy cập và quản lý trực tiếp tài nguyên phần cứng.
- Cấu trúc: `Phần cứng vật lý` -> `Type 1 Hypervisor` -> `Các Máy ảo (Guest OS)`.
- Ứng dụng: Thường được sử dụng trong môi trường doanh nghiệp, trung tâm dữ liệu để ảo hóa máy chủ (server virtualization).
- Ví dụ: VMware vSphere ESXi, Microsoft Hyper-V (trên Windows Server), Citrix XenServer, Xen (một Type 1 Hypervisor mã nguồn mở).

**Hypervisor loại 2 (Hosted Hypervisor):**

- Được cài đặt trên một hệ điều hành máy chủ (Host OS) hiện có, giống như một ứng dụng phần mềm thông thường.
- Cấu trúc: `Phần cứng vật lý` -> `Hệ điều hành máy chủ (Host OS)` -> `Type 2 Hypervisor` -> `Các Máy ảo (Guest OS)`.
- Ứng dụng: Thường được sử dụng trên máy tính cá nhân, máy trạm để phát triển, kiểm thử, hoặc học tập.
- Ví dụ: VMware Workstation, Oracle VirtualBox.

## III. Các loại ảo hóa

### 1. Ảo hóa máy chủ (Server Virtualization)

![Server Virtualization](./images/server_virtualization.png)

**Ảo hóa máy chủ (Server Virtualization)** là công nghệ tạo nhiều máy chủ ảo trên một máy chủ vật lý duy nhất thông qua phần mềm ảo hóa (hypervisor). Mỗi máy chủ ảo hoạt động độc lập, chạy hệ điều hành và ứng dụng riêng, sử dụng tài nguyên phần cứng được phân bổ (CPU, RAM, lưu trữ, mạng).

**Đặc điểm:**

- **Hypervisor quản lý:** Phân chia tài nguyên phần cứng giữa các máy ảo.
- **Tách biệt:** Các máy ảo hoạt động độc lập, không ảnh hưởng lẫn nhau.
- **Tối ưu hóa:** Tăng hiệu suất sử dụng phần cứng, giảm số lượng máy chủ vật lý.

### 2. Ảo hóa mạng (Network Virtualization)

![Network Virtualization](./images/network_virtualization.png)

**Ảo hóa mạng (Network Virtualization)** là công nghệ tạo ra các mạng ảo độc lập trên cùng một cơ sở hạ tầng mạng vật lý. Nó tách biệt các tài nguyên mạng (băng thông, địa chỉ IP, thiết bị mạng) thông qua phần mềm, cho phép mỗi mạng ảo hoạt động riêng biệt với cấu hình, chính sách và dịch vụ riêng.

**Đặc điểm:**

- **Hypervisor hoặc phần mềm quản lý:** Tạo và quản lý mạng ảo, phân bổ tài nguyên mạng.
- **Tách biệt logic:** Các mạng ảo độc lập, không xung đột, dù chạy trên cùng phần cứng.
- **Mô phỏng thiết bị mạng:** Cung cấp switch, router, firewall ảo hóa.

### 3. Ảo hóa lưu trữ (Storage Virtualization)

**Ảo hóa lưu trữ (Storage Virtualization)** là công nghệ tổng hợp nhiều thiết bị lưu trữ vật lý thành một hệ thống lưu trữ ảo thống nhất, được quản lý thông qua phần mềm.

**Đặc điểm:**

- **Lớp trừu tượng:** Phần mềm ảo hóa tạo ra một giao diện logic, che giấu sự phức tạp của các thiết bị lưu trữ vật lý.
- **Tích hợp đa dạng:** Kết hợp ổ cứng, SSD, NAS, SAN từ nhiều nhà cung cấp thành một nhóm lưu trữ chung.
- **Quản lý tập trung:** Điều khiển phân bổ, sao lưu, và di chuyển dữ liệu qua một giao diện duy nhất.

### 4. Ảo hóa hệ điều hành (Operating System Virtualization)

Cho phép chạy nhiều hệ điều hành trên cùng một nhân (kernel) duy nhất. Các hệ điều hành này được gọi là containers.

Containerization: Docker, LXC (Linux Containers).

### 5. Ảo hóa ứng dụng (Application Virtualization)

![Application Virtualization](./images/application_virtualization.png)

**Ảo hóa ứng dụng (Application Virtualization)** là công nghệ tách biệt ứng dụng khỏi hệ điều hành, cho phép ứng dụng chạy trong môi trường ảo độc lập trên máy tính hoặc máy chủ.

**Đặc điểm:**

- **Môi trường ảo:** Ứng dụng được đóng gói với các thành phần cần thiết (thư viện, cấu hình) và chạy trong một container ảo.
- **Tách biệt:** Không cài đặt trực tiếp lên hệ điều hành, tránh xung đột với các ứng dụng khác.
- **Triển khai linh hoạt:** Ứng dụng có thể chạy trên nhiều thiết bị mà không cần cài đặt riêng.

### 6. Ảo hóa máy tính (Desktop Virtualization)

![Desktop Virtualization](./images/desktop_virtualization.png)

**Ảo hóa máy tính (Desktop Virtualization)** là công nghệ cung cấp môi trường máy tính để bàn ảo, chạy trên máy chủ từ xa thay vì thiết bị vật lý của người dùng. Mỗi máy tính để bàn ảo hoạt động độc lập, mô phỏng hệ điều hành và ứng dụng, được truy cập qua mạng từ các thiết bị như PC, laptop, hoặc thiết bị mỏng (thin client).

**Đặc điểm:**

- **Chạy trên máy chủ:** Hypervisor hoặc phần mềm ảo hóa (VD: VMware Horizon, Citrix Virtual Desktops) quản lý nhiều máy tính để bàn ảo trên máy chủ trung tâm.
- **Truy cập từ xa:** Người dùng kết nối đến máy tính ảo qua giao thức mạng (RDP, VDI).
- **Tách biệt:** Mỗi máy tính ảo độc lập, có hệ điều hành, ứng dụng riêng.

### 7. Ảo hóa dữ liệu (data Virtualization)

![Data Virtualization](./images/data_virtualization.png)

**Ảo hóa dữ liệu (Data Virtualization)** là công nghệ tích hợp và cung cấp dữ liệu từ nhiều nguồn khác nhau (cơ sở dữ liệu, API, tệp, đám mây) dưới dạng một giao diện ảo thống nhất mà không cần sao chép hoặc di chuyển dữ liệu vật lý.

**Đặc điểm:**

- **Lớp ảo hóa:** Phần mềm tạo ra một lớp trừu tượng, tổng hợp dữ liệu từ các nguồn dị biệt.
- **Truy cập thời gian thực:** Cung cấp dữ liệu tức thì mà không cần lưu trữ vật lý trung gian.
- **Tích hợp linh hoạt:** Kết nối các hệ thống khác nhau (SQL, NoSQL, API) thành một nguồn dữ liệu logic.

### 8. Ảo hóa thiết bị I/O

**Ảo hóa thiết bị I/O (I/O Virtualization)** là công nghệ cho phép nhiều máy ảo chia sẻ và sử dụng các thiết bị đầu vào/đầu ra (I/O) vật lý (như card mạng, thiết bị lưu trữ, GPU) trên một máy chủ vật lý thông qua lớp ảo hóa.

**Đặc điểm:**

- **Lớp trung gian:** Hypervisor hoặc phần mềm ảo hóa (VD: SR-IOV, VMware vSphere) quản lý và phân bổ tài nguyên I/O giữa các máy ảo.
- **Mô phỏng hoặc trực tiếp:** Có thể mô phỏng thiết bị I/O ảo hoặc cung cấp truy cập trực tiếp (pass-through) đến phần cứng.
- **Chia sẻ tài nguyên:** Nhiều máy ảo sử dụng cùng thiết bị I/O mà không xung đột.

## IV. Các mức độ ảo hóa

### 1. Ảo hóa toàn phần (Full Virtualization)

**Ảo hóa toàn phần (Full Virtualization)** là công nghệ ảo hóa mô phỏng hoàn toàn phần cứng của máy tính, cho phép máy ảo chạy hệ điều hành và ứng dụng mà không cần chỉnh sửa, như một máy vật lý thực thụ.

**Đặc điểm:**

- **Mô phỏng toàn bộ:** Hypervisor tạo môi trường phần cứng ảo (CPU, RAM, ổ cứng, mạng) cho mỗi máy ảo.
- **Tương thích cao:** Hỗ trợ hầu hết hệ điều hành (Windows, Linux, v.v.) mà không cần thay đổi mã nguồn.
- **Độc lập:** Máy ảo hoạt động hoàn toàn tách biệt với hệ thống chủ.

**Lợi ích:**

- Chạy nhiều hệ điều hành khác nhau trên cùng phần cứng.
- Dễ triển khai, không cần tùy chỉnh hệ điều hành khách.

**Nhược điểm:**

- Hiệu suất thấp hơn do mô phỏng toàn bộ phần cứng.
- Tiêu tốn tài nguyên hơn so với các phương pháp khác.

### 2. Ảo hóa bán phần (Paravirtualization)

**Ảo hóa bán phần (Para-Virtualization)** là công nghệ ảo hóa trong đó máy ảo sử dụng hệ điều hành được chỉnh sửa để tương tác trực tiếp với hypervisor, thay vì mô phỏng toàn bộ phần cứng.

**Đặc điểm:**

- **Hệ điều hành chỉnh sửa:** Hệ điều hành khách cần được tối ưu hóa để nhận biết hypervisor, sử dụng các driver hoặc API đặc biệt (như VirtIO trong KVM).
- **Tương tác trực tiếp:** Máy ảo giao tiếp với hypervisor, giảm chi phí mô phỏng phần cứng.
- **Hiệu suất cao:** Ít tài nguyên hơn so với ảo hóa toàn phần.

**Lợi ích:**

- Hiệu suất gần với máy vật lý, đặc biệt cho I/O (mạng, lưu trữ).
- Tối ưu tài nguyên CPU và RAM.
- Phù hợp cho môi trường doanh nghiệp yêu cầu hiệu năng cao.

**Nhược điểm:**

- Yêu cầu chỉnh sửa hệ điều hành khách, không hỗ trợ mọi hệ điều hành.
- Phức tạp hơn trong triển khai so với ảo hóa toàn phần.

### 3. Ảo hóa hỗ trợ phần cứng (Hardware-Assisted Virtualization)

**Ảo hóa hỗ trợ phần cứng (Hardware-Assisted Virtualization)** là công nghệ ảo hóa sử dụng các tính năng tích hợp trong phần cứng (như Intel VT-x, AMD-V) để hỗ trợ máy ảo truy cập trực tiếp tài nguyên phần cứng, thay vì mô phỏng hoàn toàn.

**Đặc điểm:**

- **Hỗ trợ từ CPU:** Phần cứng (CPU) cung cấp các lệnh đặc biệt để hypervisor quản lý máy ảo hiệu quả.
- **Truy cập trực tiếp:** Máy ảo sử dụng CPU, RAM, và I/O gần giống máy vật lý, giảm chi phí mô phỏng.
- **Hệ điều hành không cần chỉnh sửa:** Hỗ trợ mọi hệ điều hành mà không cần tối ưu hóa.

**lợi ích:**

- Hiệu suất cao, gần với máy vật lý.
- Tương thích rộng, không yêu cầu chỉnh sửa hệ điều hành khách.
- Tối ưu tài nguyên, phù hợp cho môi trường doanh nghiệp và đám mây.

**Nhược điểm:**

- Phụ thuộc vào phần cứng hỗ trợ (CPU phải có Intel VT-x hoặc AMD-V).
- Yêu cầu cấu hình hypervisor phù hợp.

### 4. Ảo hóa song song (Hybrid Virtualization)

**Ảo hóa song song (Hybrid Virtualization)** là sự kết hợp giữa **ảo hóa toàn phần (Full Virtualization)** và **ảo hóa hỗ trợ phần cứng (Hardware-Assisted Virtualization)** hoặc **ảo hóa bán phần (Para-Virtualization)** để tối ưu hiệu suất và tính tương thích.

**Đặc điểm:**

- **Kết hợp kỹ thuật:** Sử dụng mô phỏng phần cứng (full virtualization) cho hệ điều hành không hỗ trợ chỉnh sửa, đồng thời tận dụng công nghệ phần cứng (Intel VT-x, AMD-V) hoặc driver para-virtualized (như VirtIO) để tăng hiệu suất.
- **Linh hoạt:** Hypervisor chuyển đổi giữa chế độ toàn phần và hỗ trợ phần cứng/bán phần tùy theo hệ điều hành khách.
- **Hiệu suất cân bằng:** Kết hợp hiệu suất cao của ảo hóa hỗ trợ phần cứng với khả năng tương thích của ảo hóa toàn phần.

**Lợi ích:**

- Hiệu suất tốt hơn ảo hóa toàn phần, gần với máy vật lý.
- Hỗ trợ đa dạng hệ điều hành mà không cần chỉnh sửa nhiều.
- Phù hợp cho môi trường hỗn hợp (các hệ điều hành cũ và mới).

**Nhược điểm:**

- Phức tạp hơn trong cấu hình hypervisor.
- Phụ thuộc vào phần cứng hỗ trợ (như Intel VT-x, AMD-V).
