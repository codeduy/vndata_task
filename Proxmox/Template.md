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
  * **Snapshot**: là chế độ backup mà ở đó sẽ tiến hành backup dữ liệu trong disk mà VM vẫn có thể hoạt động bình thường. Vì thế sẽ có rủi ro nhỏ về độ nhất quán của dữ liệu (các app vẫn tiến hành ghi dữ liệu vào disk bình thường trong khi backup nên "thời gian backup hoàn thành" của mỗi phần dữ liệu sẽ khác nhau - ví dụ như dữ liệu của 2 apps có liên quan mật thiết trong quá trình hoạt động, mà trong quá trình backup thì 2 apps đều đang ghi dữ liệu vào disk; thời gian hoàn thành backup của 2 apps khác nhau, nên dữ liệu của 2 apps đó trong bản backup sẽ không khớp, đồng bộ với nhau như ở thời điểm 2 apps đó ghi dữ liệu vào disk). Khi VM có cài đặt **Qemu Guest Agent** thì khi backup với mode này sẽ chạy 2 lệnh **guest-fsfreeze-freeze** and **guest-fsfreeze-thaw** giúp đóng băng các tiến trình có chọn lọc trong khoảng thời gian rất ngắn để chụp trạng thái dữ liệu ở block level - đảm bảo tính nhất quản của dữ liệu mà vẫn đảm bảo VM vận hành với khả năng downtime thấp nhất.
    ![](../Proxmox/images/Template/Backup_Restore/1_Backup_SnapshotMode.png)
  * **Suspend**: ở mode này thì sẽ suspend - tạm dừng toàn bộ VM ở 1 khoảng thời gian ngắn để chụp trạng thái của VM Disk ở block Layer, trước khi gọi **Snapshot** mode. Sẽ mất khoảng thời gian để tạm dừng hoàn toàn VM, vì thế nên dùng **Snapshot** để backup VM sẽ tối ưu hơn (tối ưu hơn về downtime, thời gian backup; còn về độ nhất quán dữ liệu thì không khác biệt gì so với **Snapshot**).
    ![](../Proxmox/images/Template/Backup_Restore/2_Backup_SuspendMode.png)
  * **Stop**: đây là mode gây rủi ro downtime cao nhất nhưng độ nhất quán của dữ liệu cao nhất trong 3 modes. Khi bắt đầu backup VM với mode này thì **Qemu** sẽ shutdown VM - tạm dừng toàn bộ các tiến trình (tạm dừng việc đọc/ghi dữ liệu vào disk), sau đó sẽ tự động bật lại VM để chụp trạng thái dữ liệu của disk ở block layer -> bắt đầu quá trình Backup(khi này VM sẽ hoạt động bình thường lại hoặc không tùy thuộc vào trạng thái của VM trước khi backup).
    ![](../Proxmox/images/Template/Backup_Restore/3_Backup_StopMode.png)
* **Restore**:
  * Để restore bản backup vào VM thì cần click chọn VM cần restore -> chọn **Backup** -> chọn bản backup muốn restore cho VM -> bấm **Restore**
    ![](../Proxmox/images/Template/Backup_Restore/1_Restore_Click.png)
  * Sau đó sẽ hiện 1 dialog form
    ![](../Proxmox/images/Template/Backup_Restore/2_Restore_Dialog.png)
    * Ở phần này ta có thể điều chỉnh các trường như sau:
      * **Storage**: chọn **Storage** đích - nơi chứa **Hard Disk** của VM sau khi restore
      * **Bandwidth Limit**: giới hạn băng thông - tốc độ ghi dữ liệu xuống disk mới
      * **Unique**: nếu tick tùy chọn này thì sẽ generate mới lại các thông tin riêng biệt của VM như uuid, MAC Address,...
      * **Start after restore**: nếu tick tùy chọn này thì sẽ tự đông bật lại VM sau khi restore hoàn thành.
      * **Override Settings**: tập hợp các tùy chọn điều chỉnh lại cấu hình VM sau khi hoàn thành restore.
        * **Name**: thay đổi tên của VM
        * **Memory**: thay đổi bộ nhớ **RAM** cấp phát cho  VM
        * **Cores**, **Sockets**: thay đổi lại số nhân **CPU** cấp phát cho VM
