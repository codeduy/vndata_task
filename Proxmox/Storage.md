# Storage trong Proxmox

## Sơ lược
* Về phạm vi lưu trữ chia thành 2 loại
  * **Local Storage**: là các disk được gắn/nhận dạng vào node proxmox và chỉ được sử dụng bởi các VM/Container trong chính node proxmox đó. Khi di chuyển VM sang disk khác thì ta dùng tính năng **Move Storage**.
    ![](../Proxmox/images/Storage/1_MoveStorageView.png)
  * **Shared Storage**: bao gồm nhiều disks gộp lại thành một disk logic mà cluster nhìn thấy - cấp phát cho các node proxmox sử dụng. Dạng storage này có phần nổi trội hơn **Local Storage** bởi việc **live-migrate** được thực hiện gần như tức thì - disk của VM nằm trong **Shared Storage** mà cluster (nhiều node proxmox) có thể truy cập trực tiếp được; vì thế, khi thực hiện live-migrate giữa các node proxmox thì chỉ cần chuyển trạng thái RAM, CPU.  
* Có hai cấp độ lưu trữ mà Proxmox hỗ trợ: bao gồm **File level storage** và **Block level storage**. Với mỗi loại lưu trữ như **ZFS, LVM, Directory, LVM-thin,...** đều được proxmox định nghĩa/điều khiển nhờ vào gói **libpve-storage-perl**  - gói này bao gồm các plugin/module định nghĩa cho từng loại lưu trữ **(zfspool, lvm, dir, lvmthin,...)**.
  ![](../Proxmox/images/Storage/2_StoragePluginList.png)
* **Thin Provisioning** là tính năng mà ở đó VM sẽ chỉ sử dụng phần disk mà VM thực sự đang dùng (ví dụ: nếu cấp phát cho VM 30GB Disk, mà OS + App chỉ sử dụng 5GB, thì 25GB còn lại có thể được sử dụng bởi các VM khác. Ngược lại, thì sẽ cấp phát đầy đủ 30GB Disk chỉ cho VM đó).
  > Cần lưu ý đảm bảo để trống vùng disk phù hợp với nhu cầu ghi dữ liệu của các VM, tránh hiện tượng **over-provisioning** - là hiện tượng mà VM ghi dữ liệu thỏa mản dung lượng disk tối đa được cấp phát nhưng lại không thoả lượng disk thực tế còn trống gây hỏng dữ liệu.
> Tất cả các cấu hình liên quan đến storage đều được lưu trữ ở file **/etc/pve/storage.cfg**
> ![](../Proxmox/images/Storage/3_StorageConfigText.png)
* Về các thuộc tính chính của storage
  * **ID**: tên định danh duy nhất của storage được proxmox nhận dạng trong **/etc/pve/storage.cfg**
  * **Content**: các loại dữ liệu mà loại storage đó hỗ trợ lưu trữ.
    > * Với loại storage là **Directory** thì hỗ trợ **file level** nên sẽ support các dạng lưu trữ như hình
    > ![](../Proxmox/images/Storage/4_Storage_FileLevel.png)
    > * Đối với **LVM-Thin** - chỉ hỗ trợ **block level** nên chỉ có thể support **Disk image** và **Container**
    > ![](../Proxmox/images/Storage/5_Storage_BlockLevel.png)
  * **Nodes**: chỉ các **proxmox node** có quyền tương tác/đọc - ghi dữ liệu đối với storage tương ứng
  * **Enable**: khi bỏ tick tùy chọn này thì sẽ ngắt mọi giao tiếp từ thao tác trên giao diện proxmox liên quan đến **Hard Disk** thực thi lên storage tương ứng.
    > ![](../Proxmox/images/Storage/6_Enabled_NO.png)
    > * VM ở trạng thái **running**: sẽ không thể tương tác với disk thuộc Storage đã bỏ tick **Enabled** như **Resize** (báo lỗi như hình), **Add** Hard Disk (không hiện disk để **Add**); ở trong VM thì vẫn có thể thực hiện các thao tác sao chép, di chuyển, tải dữ liệu từ Internet bình thường.
    >   ![](../Proxmox/images/Storage/7_Storage_ResizeDisks.png)
    >   ![](../Proxmox/images/Storage/8_Storage_CantAddHardDisk.png)
    > * VM ở trạng thái **stop**, **shutdown** thì sẽ không thể **Start** lại được nữa, và sẽ báo lỗi như hình - **proxmox node** không còn quyền đọc/ghi dữ liệu của VM Disk để khởi động VM
    >   ![](../Proxmox/images/Storage/10_Storage_StartVMFail.png)
    > * Ở phía giao diện **Shell** của proxmox node thì vẫn có thể thao tác đọc/ghi dữ liệu bình thường
    >   ![](../Proxmox/images/Storage/9_Storage_WgetCommand.png)
    

> 
>
> ZFS: cần nắm rõ 3 định nghĩa về ZPOOL(bao gồm các RAID tương tự như LVM), Checksum (kiểm tra tính toàn vẹn của dữ liệu lúc đọc và ghi), CoW (Copy on Write - cần đảm bảo pool còn trống để ghi metadata mới ghi xóa dữ liệu)
> ZFS có thể lưu cả dữ liệu ở file level và block level
> Yêu cầu về hardware: ZFS cần nhiều RAM để xử lí hơn so với LVM; SSD
> 
> Các loại RAID và ưu/nhược điểm từng loại
>
> Demo và đối chiếu resize giảm disk đối với disk .qcow2 và .raw


## LVM, ZFS

### LVM
* Có ba định nghĩa cốt lõi:
  * **Physical Volumes (PV)**: là các thiết bị lưu trữ như ổ đĩa SSD/HDD được **LVM** đánh dấu dùng cho việc quản lí.
  * **Volume Group (VG)**: gộp tất cả các **PV** thành một ổ thống nhất mà OS nhìn thấy gọi là **VG**.
  * **Logical Volume (LV)**: là các "mảnh" phân vùng lấy từ **VG** để cấp phát cho các VM, các VM nhận dạng **LV** như các ổ đĩa ảo để lưu trữ dữ liệu
  > Trong **LVM**, nếu một ổ bị hỏng thì sẽ làm hỏng toàn bộ dữ liệu nằm trên các ổ còn lại - bởi vì OS + VM xem **LV** là một ổ thống nhất.
* Sơ đồ tổng quát:
  
![](../Proxmox/images/Storage/11_Storage_LVMDiagram.png)

### LVM-Thin
* Cũng là **LVM** nhưng có hỗ trợ **thin provisioning** thông qua việc tạo thêm **Thin Pool** từ **VG** - để tạo được **LVM-Thin** thì cần phải có **VG** từ **LVM**
  > * **Thin Pool** tồn tại trên block-level storage là để hình thành cơ chế **thin provisioning** mà block-level storage không có (VM sẽ lấy disk ảo từ **Thin Pool** để làm virtual **Hard Disk** cho chính VM đó)
  > * **Thin provisioning** là tính năng giúp tiết kiệm dung lượng disk - phần disk đã cấp phát cho VM mà VM đó chưa dùng thì có thể được sử dụng bởi VM khác.
  >   * Ví dụ: Cấp phát 100GB cho Linux VM nhưng chỉ dùng có 10GB thì 90GB còn lại có thể dùng cho VM khác, Linux VM đó vẫn có thể sinh dữ liệu trong quá trình vận hành cho tới khi đạt mức giới hạn tối đa đã cấp phát là là 100GB.
  >   * Vì thế, sẽ gây nên rủi ro gọi là **over-provisioning** - đặt ở trường hợp Linux VM trên, VM nhìn thấy còn trống 90GB mà disk thực tế còn trống chỉ có 10GB, thì khi Linux VM trên ghi 20GB dữ liệu vào disk sẽ làm dữ liệu có khả năng bị hỏng/ mất tính nhất quán của dữ liệu -> Cần monitor storage (cảnh báo khi disk đạt 80-90% tùy vào dung lượng disk/ mức độ tương tác thường xuyên của hệ thống lên disk đó) để tránh **over-provisioning**.
* Ta có thể tạo nhiều **Thin Pool** từ **VG** để quản lí rủi ro tùy theo nhu cầu - mục đích. Ví dụ, ta có thể tạo hai **Thin Pool**, một pool để chạy các VM trong môi trường production, một pool dùng cho mục đích test/kiểm thử; nếu pool dùng để test/kiểm thử bị **over-provisioning** thì sẽ không ảnh hưởng đến pool dùng để cấp phát cho VM chạy trong môi trường production.
* Các loại storage có hỗ trợ **thin provisioning** đều hỗ trợ **Snapshot**, nhưng chỉ với VM lưu **Hard Disk** ở định dạng **.qcow2**, còn ở định dạng **.raw** thì không.
* Sơ đồ tổng quát:

![](../Proxmox/images/Storage/12_Storage_LVMThinDiagram.png)

> ![](../Proxmox/images/Storage/13_Storage_Shell.png)

### Demo tạo Storage LVM, LVM-Thin trên file giả lập
> test với tạo VM .raw; import disk với định dạng .qcow2 (sẽ fail); kiểm thử snapshot với VM trên LVM (không thể dùng snapshot tích hợp sẵn trên giao diện được) và LVMThin
>
> Phân biệt **Snapshots** và **Backup** - **Snapshot** type



### ZFS

