# Tìm hiểu các chế độ card mạng trong KVM

**KVM** bản chất không quy định cứng "có 3 loại card mạng". Thay vào đó, kiểu kết nối mạng (network mode) của máy ảo **KVM** phụ thuộc vào cách cấu hình bridge, NAT, hay isolated network qua các công cụ như `libvirt`, `virt-manager`, `virsh`, hoặc `qemu-system`.

## Phân loại theo chức năng cơ bản

| Loại | Chức năng |
|------|-----------|
| **NAT** | Máy ảo truy cập Internet qua IP của host (giống NAT router) |
| **Host-only** | Chỉ giao tiếp với host, không có internet |
| **Bridged** | Máy ảo nằm chung mạng với host và các thiết bị khác |

>Đây là cách phân loại thường thấy trong VirtualBox, VMware, và nhiều tài liệu tổng quát

## Phân loại theo ngữ cảnh mạng Linux bridge (libvirt dùng)

| Loại | Chức năng | Thường thấy trong |
|------|-----------|-----------|
| **NAT** | Mặc định (default network), có `virbr0`, kết nối ra Internet qua host | `virt-manager`, `libvirt` |
| **Private bridged** | Máy ảo giao tiếp nội bộ, không ra Internet, bridge nội bộ riêng | Một số tài liệu KVM nâng cao |
| **Public bridged** | Gắn trực tiếp vào bridge kết nối với card mạng thật, máy ảo lấy IP như máy vật lý | Khi dùng `br0`, `br1` gắn với `eth0` thật |
