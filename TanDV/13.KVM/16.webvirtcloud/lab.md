# Các thao tác với VM trên webvirtcloud

## I. Thêm VM

Tạo 1 storage để chạy VM, chọn **Computes**:

![web1](./images/web1.png)

Tại giao diện **Computes**, chọn 1 máy để storage (click vào biểu tượng **view** của máy đó):

![web2](./images/web2.png)

Trong giao diện của máy vừa chọn, chọn tab **Storages**:

![web3](./images/web3.png)

Tại đây, chọn 1 mục để phân vùng lưu trữ VM, ví dụ **default**:

![web4](./images/web4.png)

Click chọn biểu tượng **Add Volume** để thêm volume:

![web5](./images/web5.png)

Điền thông tin cho VM, sau đó click **Create**:

![web6](./images/web6.png)

Nếu thành công, sẽ nhận được thông báo như hình:

![web7](./images/web7.png)

Tiếp đến, chọn **Instances**, sau đó click vào biểu tượng **Create Instance** để tạo VM:

![web8](./images/web8.png)

Chọn 1 **Compute (host)** để tạo VM, ở đây có 1 **Compute** là **vm1**:

![web9](./images/web9.png)

Chọn cấu hình máy ảo để tạo hoặc tạo bằng `XML`:

![web10](./images/web10.png)

Tại màn tiếp theo, chọn **cấu hình có sẵn (Flavor)** hoặc **tự cấu hình (Custom)**:

![web11](./images/web11.png)

Với chọn cấu hình sẵn, sau khi chọn biểu tượng dấu **+** sẽ xuất hiện thông tin cấu hình. Thêm thông tin và **Create** để tạo VM:

![web12](./images/web12.png)

Với cấu hình tùy chỉnh, điền thông tin và **Create** để tạo VM:

![web13](./images/web13.png)

## II. Sửa VM

Để thay đổi thông số VM, chọn tab **Instances**, sau đó click vào tên của VM cần sửa:

![web14](./images/web14.png)

Chọn tab **Resize** và thay đổi thông số VM (CPU, Memory, Disk), nhấn **Resize** để lưu lại:

![web15](./images/web15.png)

## III. Clone VM

Để clone VM, chọn **Instances** -> **Clone** -> Thêm tên mới cho VM clone -> **Clone**:

![web16](./images/web16.png)

Sau khi clone thành công, hiển thị giao diện VM đã clone:

![web17](./images/web17.png)

## IV. Snapshot VM

### 1. Tạo Snapshot

Cần tắt VM trước khi snapshot.

Chọn **Snapshot** -> Điền thông tin -> **Take Snapshot**:

![web18](./images/web18.png)

Sau khi tạo snapshot thành công, có thể thấy snapshot trong tab **Manage Snapshots**:

![web19](./images/web19.png)

### 2. Quay lại Snapshot

Click vào biểu tượng **Revert** trong bản snapshot muốn quay lại để quay lại snapshot:

![web20](./images/web20.png)

### 3. Xóa Snapshot

Click vào biểu tượng **Delete** trong bản snapshot muốn xóa để xóa snapshot (Biểu tượng thùng rác đỏ bên canh biểu tượng **Revert**).

## V. Xóa VM

Chọn VM muốn xóa, sau đó chọn tab **Destroy** -> **Destroy** để xóa VM:

![web21](./images/web21.png)
