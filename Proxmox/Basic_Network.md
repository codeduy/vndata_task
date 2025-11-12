# Cấu hình Network - Firewall cơ bản cho Proxmox - VM

## Cấu hình bridge Network cho VM ra Internet 
Ở phần này ta cần gán NIC **vmbr0** đã tạo ở ![Cấu hình Network](https://github.com/codeduy/vndata_task/blob/main/Proxmox/NodeSetup.md#c%E1%BA%A5u-h%C3%ACnh-network) vào VM theo các bước sau:
* Click chọn phần **Hardware** --> **Network Device (net0)** --> **Edit**
  ![](../Proxmox/images/Network_Basic/Edit_NetworkDevice.png)
* Chọn bridge **vmbr0** tại phần **Bridge** và chọn **OK**
  ![](../Proxmox/images/Network_Basic/Bridge_SelectStep.png)
* Reboot VM để áp dụng cấu hình mạng

## Cấu hình network private giữa các VM
### Tạo linux bridge vmbr1 gắn dải ip private -> cấp phát cho các VM
* Truy cập vào phần **Network** của node **proxmox** để bắt đầu tạo linux bridge
  ![](../Proxmox/images/Network_Basic/Createvmbr1.png)
* Điền thông tin của **vmbr1** và bấm **Create** để tạo linux bridge **vmbr1**
  ![](../Proxmox/images/Network_Basic/Savevmbr1.png)
  ![](../Proxmox/images/Network_Basic/vmbr1_view.png)
* Tiếp theo, ta cần gắn NIC vmbr1 vào VM với các bước tương tự như ở phần 
