# Cloud-init trong Proxmox

## Sơ lược
* Cloud-Init là một tính năng có sẵn trong Proxmox, có thể cấu hình để giao tiếp với Cloud-Init trên Linux và Cloudbase-Init trên window thông qua một ổ CD Drive ảo; hữu ích cho việc thay đổi username, password, cấu hình mạng,... của VM mà không cần phải truy cập trực tiếp vào **Console**.

## Tạo template Linux Ubuntu tích hợp Cloud-Init

### Khởi tạo VM từ Ubuntu Cloud image được cài sẵn dịch vụ cloud-init
* Chọn **Create VM** để bắt đầu khởi tạo VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/1_LinuxCloudInit_ClickCreateVM.png)
* Điều chỉnh **Name** cho VM và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/2_LinuxCloudInit_VMGeneral.png)
* Ta sẽ import image ở định dạng **.qcow2** sau, vì thế ta chọn **Do not use any media** và click **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/3_LinuxCloudInit_VMOS.png)
* Click chọn **Qemu Agent** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/4_LinuxCloudInit_VMSystem.png)
* Ta sẽ import Disk kèm với image **.qcow2** -> ở phần này xóa Disk mặc định
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/5_LinuxCloudInit_VMDisks.png)
* Sau đó bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/6_LinuxCloudInit_RemoveDisk.png)
* Có thể tùy chỉnh nhân CPU cho VM tùy theo nhu cầu ở phần **Cores** -> bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/7_LinuxCloudInit_CPU.png)
* Có thể tùy chỉnh bộ nhớ RAM cho VM tại **Memory** -> bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/8_LinuxCloudInit_Memory.png)
