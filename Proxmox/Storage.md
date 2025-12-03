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
    
> LVM: có 3 định nghĩa cần nắm là physical volume, volume group, logical volume
> Ở volume group, nếu một ổ cứng bị hỏng thì sẽ làm hỏng toàn bộ dữ liệu nằm trên volume group đó; do ở khía cạnh phần mềm thì các app/OS trên lớp trên coi volume group đó là một virtual disk thống nhất.
>
> LVM-thin
>
> ZFS: cần nắm rõ 3 định nghĩa về ZPOOL(bao gồm các RAID tương tự như LVM), Checksum (kiểm tra tính toàn vẹn của dữ liệu lúc đọc và ghi), CoW (Copy on Write - cần đảm bảo pool còn trống để ghi metadata mới ghi xóa dữ liệu)
> ZFS có thể lưu cả dữ liệu ở file level và block level
>
> Tại sao có thin-pool trên block-level storage
> Các loại RAID và ưu/nhược điểm từng loại


