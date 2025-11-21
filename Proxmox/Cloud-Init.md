# Cloud-init trong Proxmox

## Sơ lược
* Cloud-Init là một tính năng có sẵn trong Proxmox, có thể cấu hình để giao tiếp với Cloud-Init trên Linux và Cloudbase-Init trên Windows thông qua một ổ CD Drive ảo; hữu ích cho việc thay đổi username, password, cấu hình mạng,... của VM mà không cần phải truy cập trực tiếp vào **Console**.

## Tạo template Linux Ubuntu tích hợp Cloud-Init

### Khởi tạo VM từ Ubuntu Cloud image được cài sẵn dịch vụ cloud-init
* Chọn **Create VM** để bắt đầu khởi tạo VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/1_LinuxCloudInit_ClickCreateVM.png)
* Điều chỉnh **Name** cho VM và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/2_LinuxCloudInit_VMGeneral.png)
* Ta sẽ import image ở định dạng **.img** sau, vì thế ta chọn **Do not use any media** và click **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/3_LinuxCloudInit_VMOS.png)
* Click chọn **Qemu Agent** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/4_LinuxCloudInit_VMSystem.png)
* Ta sẽ import Disk kèm với image **.img** -> ở phần này xóa Disk mặc định
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/5_LinuxCloudInit_VMDisks.png)
* Sau đó bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/6_LinuxCloudInit_RemoveDisk.png)
* Có thể tùy chỉnh nhân CPU cho VM tùy theo nhu cầu ở phần **Cores** -> bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/7_LinuxCloudInit_CPU.png)
* Có thể tùy chỉnh bộ nhớ RAM cho VM tại **Memory** -> bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/8_LinuxCloudInit_Memory.png)
* Chọn bridge tại **Bridge** -> bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/9_LinuxCloudInit_Network.png)
* Chọn **Finish**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/10_LinuxCloudInit_Confirm.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/11_LinuxCloudInit_VMSummary.png)
* Ta cần tải file image ở định dạng **.img** tại [Ubuntu Cloud-Image](https://cloud-images.ubuntu.com/jammy/current/?C=M;O=A)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/12_LinuxCloudInit_CloudImageDownloading.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/13_LinuxCloudInit_CloudImageDownloaded.png)
* Import file image đã tải ở trên thành Disk cho VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/14_LinuxCloudInit_CreateNewVolumeDisk.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/15_LinuxCloudInit_SuccessfullyCreateVolumeDisk.png)
* Quay trở lại trang quản trị **proxmox**, click **Hardware** -> double click vào Disk vừa mới import lúc nãy, tức **Unused Disk 0**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/16_LinuxCloudInit_DoubleClickUnusedDisk.png)
* Và chọn **Add**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/17_LinuxCloudInit_ClickAdd.png)
* Tiếp theo, ta chọn disk vừa mới set controller ở bước trước -> click **Disk Action** -> chọn **Resize**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/18_LinuxCloudInit_NewVolumeDiskAdded.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/19_LinuxCloudInit_ResizeDisk.png)
* Điều chỉnh lại dung lượng cần cấp phát thêm cho Disk tại **Size Increment**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/20_LinuxCloudInit_InputSize.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/21_LinuxCloudInit_DiskSizeView.png)
* Tiếp theo, ta cần thêm CloudInit Drive theo các bước như hình
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/22_LinuxCloudInit_AddCloudInitDrive.png)
* Chọn storage chứa **CloudInit Drive** tại **Storage** -> click **Add**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/23_LinuxCloudInit_ChooseStorage.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/24_LinuxCloudInit_DriveView.png)
* Click **Options** -> chọn phần **Boot Order** -> click **Edit**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/25_LinuxCloudInit_EditBootOrder.png)
* Click **Enabled** đối với device **scsi0** và kéo thả lên vị trí đầu tiên
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/26_LinuxCloudInit_Enable_DragandDrop.png)
* Chọn **OK**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/27_LinuxCloudInit_BootOptionsView.png)
* Ở cửa sổ **Console**, click **Start** để bật VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/28_LinuxCloudInit_StartVM.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/29_LinuxCloudInit_ConsoleView.png)
* Để có thể login được vào VM thì ta sẽ phải chỉnh **User** và **Password** ở phần **Cloud-Init**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/30_LinuxCloudInit_View.png)
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/31_LinuxCloudInit_U%26P_Modify.png)
* Sau đó, click **Regenerate Image** để áp dụng các thay đổi từ Cloud-Init lên VM -> **Reboot** VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/32_LinuxCloudInit_RebootVM.png)
* Click **Yes** để xác nhận **Reboot**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/33_LinuxCloudInit_ConfirmReboot.png)
* Sau khi **Reboot** hoàn thành thì ta có thể dùng các thông tin login tại Cloud-Init ở bước trước để login VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/34_LinuxCloudInit_LoginSuccess.png)
* Hiện tại thì VM chưa có thể truy cập ra Internet được do thiếu cấu hình mạng
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/35_LinuxCloudInit_NoNetwork.png)
* Ta sẽ cấu hình các phần liên quan tại **Cloud-Init** để kiểm thử việc kết nối mạng
  * Đầu tiên là thay đổi DNS: Click **Cloud-Init** -> Double click vào phần **DNS servers**
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/36_LinuxCloudInit_ChangeDNS.png)
  * Điền địa chỉ DNS của Google (hoặc các public DNS tùy ý muốn) vào phần **DNS servers** và click **OK**
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/37_LinuxCloudInit_DNSInput.png)
  * Tiếp theo, double click vào mục **IP Config** để **Edit** phần này
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/38_LinuxCloudInit_DNSView.png)
  * Tùy chỉnh các trường **IPv4/CIDR** và **Gateway (IPv4)** -> click **OK**
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/39_LinuxCloudInit_NetworkConfigEdit.png)
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/40_LinuxCloudInit_NetworkConfigView.png)
  * Click **Regenerate Image** -> **Reboot** VM
    ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/41_LinuxCloudInit_VMReboot.png)
* Sau khi **Reboot** xong thì ta vào VM, kiểm tra lại file cấu hình mạng
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/42_LinuxCloudInit_VMConsolePlus.png)
* Kiểm tra kết nối Internet
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/43_LinuxCloudInit_VMNetworkCheck.png)
> Từ đây, ta có thể convert VM ra Template và clone từ Template ra các VM khác

## Tạo Template Windows Server tích hợp với Cloudbase-Init

### Khởi tạo Windows Server 2022 VM
