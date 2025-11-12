# Cấu hình Network - Firewall cơ bản cho Proxmox - VM

## Cấu hình bridge Network cho VM ra Internet 
Ở phần này ta cần gán NIC **vmbr0** đã tạo ở ![Cấu hình Network](https://github.com/codeduy/vndata_task/blob/main/Proxmox/NodeSetup.md#c%E1%BA%A5u-h%C3%ACnh-network) vào VM theo các bước sau:
* Click chọn phần **Hardware** --> **Network Device (net0)** --> **Edit**
  ![](../Proxmox/images/Network_Basic/Edit_NetworkDevice.png)
* Chọn bridge **vmbr0** tại phần **Bridge** và chọn **OK**
  ![](../Proxmox/images/Network_Basic/Bridge_SelectStep.png)
* Reboot VM để áp dụng cấu hình mạng

## Cấu hình network private giữa các VM
### Tạo linux bridge vmbr1 gắn dải ip private - cấp phát cho các VM
* Truy cập vào phần **Network** của node **proxmox** để bắt đầu tạo linux bridge
  ![](../Proxmox/images/Network_Basic/Createvmbr1.png)
* Điền thông tin của **vmbr1** và bấm **Create** để tạo linux bridge **vmbr1**
  ![](../Proxmox/images/Network_Basic/Savevmbr1.png)
  ![](../Proxmox/images/Network_Basic/vmbr1_view.png)
* Tiếp theo, ta cần gắn NIC vmbr1 vào VM với các bước tương tự như ở phần ![Cấu hình bridge Network cho VM ra Internet](https://github.com/codeduy/vndata_task/blob/main/Proxmox/Basic_Network.md#c%E1%BA%A5u-h%C3%ACnh-bridge-network-cho-vm-ra-internet)
  ![](../Proxmox/images/Network_Basic/NetworkDevice_vmbr1.png)
  ![](../Proxmox/images/Network_Basic/vmbr1attach.png)
  ![](../Proxmox/images/Network_Basic/vmbr1_vm_view.png)
* Reboot lại VM để áp dụng NIC vmbr1
* Cấu hình thủ công file cấu hình mạng trong VM để có thể giao tiếp nội bộ bên trong NIC vmbr1
  ![](../Proxmox/images/Network_Basic/netplan_vmbr1.png)

### Tạo SDN để cấp phát IP Private cho các VM
* Đầu tiên, ta cần vào phần **SDN** trong mục **Datacenter** để thêm **"Simple" Zone**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Addzone.png)
* Sau khi click chọn **Simple** thì sẽ hiện một form dialog như hình để điền các thông tin cần thiết. Sau khi điền xong thì bấm **Add**
  ![](../Proxmox/images/Network_Basic/SDN/Addzone001.png)
  ![](../Proxmox/images/Network_Basic/SDN/zone001_view.png)
* Tiếp đến, ta sẽ tạo **Vnet001** và subnet tương ứng cho **Zone001**. Đầu tiên, click **VNets** và chọn **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Vnet_Create.png)
* Sau đó, sẽ hiện một form dialog để điền thông tin của Vnet. Ta sẽ điền tên Vnet vào trường **Name**, và chọn zone tương ứng trong **Zone** và bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Vnet001_Create.png)
  ![](../Proxmox/images/Network_Basic/SDN/Vnet001_View.png)
* Tiếp tục, ta sẽ tạo subnet cho **Vnet001**; click chọn **Vnet001** và bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/Createsubnet_vnet.png)
* Và sẽ hiện dialog form để điền thông tin của subnet. Sau khi điền xong thì bấm **Create**
  ![](../Proxmox/images/Network_Basic/SDN/inputsubnet_vnet001.png)
  ![](../Proxmox/images/Network_Basic/SDN/vnet_subnet_view.png)
* Click **SDN** và bấm **Apply** để áp dụng cấu hình của **Zone001**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Apply.png)
* Click **Yes**
  ![](../Proxmox/images/Network_Basic/SDN/SDN_Yes.png)
  ![](../Proxmox/images/Network_Basic/SDN/Zone001_Available.png)
  
  
  
