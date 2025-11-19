# Template trong Proxmox

## Sơ lược về template
* Template là một VM đã cài OS + App hoàn chỉnh (aaPanel, Qemu Guest Agent, monitoring agent, OS config tùy chỉnh, trình duyệt...) --> đóng băng lại thành một template dùng để clone - triển khai thành nhiều VM; giúp tiết kiệm thời gian, giảm lỗi so với việc cấu hình thủ công từng VM.
* Có hai loại Clone là **Linked Clone** và **Full Clone**:
  * **Linked Clone**: clone thành VM dùng chung imageOS + App ở định dạng .qcow2/raw nằm trong **Hard Disk** của template. Còn **Hard Disk** của VM mới clone ra thì chỉ dùng để lưu những thay đổi của OS/App trong VM đó so với template.
    > Ở tùy chọn này thì VM sẽ không thể thực hiện live migrate disk được; nếu muốn thực hiện thì cần migrate cả phần template theo qua ổ đĩa lưu trữ tương ứng của VM.
  * **Full Clone**: sao chép toàn bộ mọi thứ từ Template qua VM mới, VM và template sẽ độc lập với nhau - không còn liên kết ở bất cứ dữ liệu nào nữa.
  > Khi clone ra VM mới thì sẽ tự động đổi các thông tin sau: VM BIOS UUID (tránh xung đột danh tính hệ thống - các app bản quyền,... cần phân biệt phần này), MAC Address (để tránh xung đột frame ARP - làm VM mất mạng).

## Tạo template và clone VM từ template

### Tạo template Linux Ubuntu 22.04
* Đầu tiên, ta cần tạo Ubuntu 22.04 VM
  * Click **Create VM**
    ![](../Proxmox/images/Template/LinuxTemplate/1_LinuxTemplate_CreateVM.png)
  * Điền tên VM vào **Name** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/2_LinuxTemplate_General.png)
  * Chọn file ISO tương ứng tại **ISO image** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/3_LinuxTemplate_OS.png)
  * Chọn **Qemu Agent** để mở một cổng giao tiếp từ **proxmox** đến **Qemu Guest Agent** trên VM -> click **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/4_LinuxTemplate_System.png)
  * Ta có thể điều chỉnh dung lượng đĩa cấp phát cho VM tại **Disk size** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/5_LinuxTemplate_Disks.png)
  * Điều chỉnh nhân CPU cấp phát cho VM tại **Cores** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/6_LinuxTemplate_CPU.png)
  * Điều chỉnh dung lượng RAM cấp phát cho VM tại **Memory** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/7_LinuxTemplate_Memory.png)
  * Chọn bridge mạng tại **Bridge** và bấm **Next**
    ![](../Proxmox/images/Template/LinuxTemplate/8_LinuxTemplate_Network.png)
  * Kiểm tra lại các thông tin VM và bấm **Finish**
    ![](../Proxmox/images/Template/LinuxTemplate/9_LinuxTemplate_Confirm.png)
    > Các VM được clone từ template sẽ mang các thông tin cấu hình tương tự như trong hình
* Click **Start Now** để bật VM
  ![](../Proxmox/images/Template/LinuxTemplate/1_LinuxTemplate_StartButtonClick.png)
  ![](../Proxmox/images/Template/LinuxTemplate/2_LinuxTemplate_InstallUbuntuServer.png)
  ![](../Proxmox/images/Template/LinuxTemplate/3_LinuxTemplate_UbuntuConsole.png)
* Ubuntu VM mặc định sẽ không cài sẵn **Qemu Guest Agent**
  ![](../Proxmox/images/Template/LinuxTemplate/4_LinuxTemplate_qemuGANotFound.png)
* Ta sẽ cài **Qemu Guest Agent**
  ![](../Proxmox/images/Template/LinuxTemplate/5_LinuxTemplate_QGAFound.png)
* Tiếp theo, cần chuyển đổi từ VM đang chạy sang Template
  * Click **Shutdown** để tắt VM
    ![](../Proxmox/images/Template/LinuxTemplate/1_LinuxTemplate_ShutdownClick)
  * Click **Yes**
    ![](../Proxmox/images/Template/LinuxTemplate/2_LinuxTemplate_Shutdown_ClickYes.png)
    ![](../Proxmox/images/Template/LinuxTemplate/3_LinuxTemplate_ShutdownView.png)
  * Tiếp theo, click **More** -> **Convert to template**
    ![](../Proxmox/images/Template/LinuxTemplate/4_LinuxTemplate_ConvertToTemplate.png)
  * Click **Yes**
    ![](../Proxmox/images/Template/LinuxTemplate/5_LinuxTemplate_ConvertToTemplate_ClickYes.png)
    ![](../Proxmox/images/Template/LinuxTemplate/6_LinuxTemplate_TemplateConverted.png)

## VM Backup & Restore
* **Backup & Restore** là tính năng được tích hợp sẵn trong **proxmox** giúp sao lưu (**Backup**) dữ liệu trong Disk của VM và khôi phục (**Restore**) bản sao lưu - hoàn tác VM về trạng thái cũ tương ứng với bản Backup
* Các loại **Backup**:
  * **Snapshot**: là chế độ backup mà ở đó sẽ tiến hành backup dữ liệu trong disk mà VM vẫn có thể hoạt động bình thường. Vì thế sẽ có rủi ro nhỏ về độ nhất quán của dữ liệu (các app vẫn tiến hành ghi dữ liệu vào disk bình thường trong khi backup nên "thời gian backup hoàn thành" của mỗi phần dữ liệu sẽ khác nhau - ví dụ như dữ liệu của 2 apps có liên quan mật thiết trong quá trình hoạt động, mà trong quá trình backup thì 2 apps đều đang ghi dữ liệu vào disk; thời gian hoàn thành backup của 2 apps khác nhau, nên dữ liệu của 2 apps đó trong bản backup sẽ không khớp, đồng bộ với nhau như ở thời điểm 2 apps đó ghi dữ liệu vào disk)
  * **Suspend**: 
