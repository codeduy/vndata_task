# Cloud-init trong Proxmox

## Sơ lược
* Cloud-Init là một tính năng có sẵn trong Proxmox, có thể cấu hình để giao tiếp với Cloud-Init trên Linux và Cloudbase-Init trên Windows thông qua một ổ CD Drive ảo; hữu ích cho việc thay đổi username, password, cấu hình mạng,... của VM mà không cần phải truy cập trực tiếp vào **Console**.

## Tạo template Linux Ubuntu tích hợp Cloud-Init

### Khởi tạo VM từ Ubuntu Cloud image được cài sẵn dịch vụ cloud-init
* Chọn **Create VM** để bắt đầu khởi tạo VM
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/1_LinuxCloudInit_ClickCreateVM.png)
* Điều chỉnh **Name** cho VM và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Linux_CloudInit/2_LinuxCloudInit_VMGeneral.png)
* Ta sẽ import image ở các bước sau ở định dạng **.img** sau, vì thế ta chọn **Do not use any media** và click **Next**
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
* Chọn network bridge tại **Bridge** -> bấm **Next**
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
* Hiện tại thì VM chưa thể truy cập ra Internet được do thiếu cấu hình mạng
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

### Khởi tạo Windows Server 2019 VM và cài đặt Cloudbase-Init
* Click **Create VM** để bắt đầu khởi tạo VM
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/1_WCI_CreateVM.png)
* Điền **Name** của VM và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/2_WCI_General.png)
* Điều chỉnh các phần **ISO image**; **Type**; **Version**; **Storage** và **ISO image** của **VirtIO drivers** nhưng trong hình 
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/3_WCI_OS.png)
* Tick **Qemu Agent** và chọn **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/4_WCI_System.png)
* Điều chỉnh phần **Disk size** cho phù hợp và chọn **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/5_WCI_Disks.png)
* Điều chỉnh nhân CPU cấp phát cho VM tại **Cores** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/6_WCI_CPU.png)
* Điều chỉnh bộ nhớ RAM cấp phát cho VM tại **Memory** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/7_WCI_Memory.png)
* Chọn **Bridge** mạng và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/8_WCI_Network.png)
* Kiểm tra lại các thông tin của VM và bấm **Finish** để khởi tạo VM
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/9_WCI_Confirm.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/10_WCI_VMSummary.png)
* Tiếp đến, ta sẽ thêm **CloudInit Drive** ở phần **Hardware** 
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/11_WCI_AddCIDrive.png)
* Chọn **Storage** chứa **CloudInit Drive** và bấm **Add**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/12_WCI_AddCIDrive.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/13_WCI.png)
* Click **Console** và bấm **Start** để khởi chạy VM
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/14_WCI_VMStart.png)
* Sau khi VM khởi động thì bắt đầu các bước cài đặt OS. Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/15_WCI_InstallWinOS_Step1.png)
* Click **Install now**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/16_WCI_InstallWinOS_Step2.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/17_WCI_InstallWinOS_Step2Waiting.png)
* Chọn loại OS như hình và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/18_WCI_InstallWinOS_Step3.png)
* Click **I accept the license terms** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/19_WCI_InstallWinOS_Step4.png)
* Chọn **Custom**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/20_WCI_InstallWinOS_Step5.png)
* Click **Load driver**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/21_WCI_InstallWinOS_Step6.png)
* Click **OK**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/22_WCI_InstallWinOS_Step7.png)
* Chọn tùy chọn VirtIO Driver như hình và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/23_WCI_InstallWinOS_Step8.png)
* Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/24_WCI_InstallWinOS_Step9.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/25_WCI_InstallWinOS_Step9Waiting.png)
* Sau khi hoàn thành quá trình cài đặt thì sẽ tự động reboot lại OS
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/26_WCI_InstallWinOS_Step9_Reboot.png)
* Tiếp theo, ta nhập password và bấm **Finish**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/28_WCI_InstallWinOS_Step10_InputPassword.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/29_WCI_InstallWinOS_Completed.png)
* Nhập password và login vào OS
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/30_WCI_InputPassword_Submit.png)
* Sau khi login thành công thì sẽ tự động khởi chạy **Server Manager** và hiện message như hình, có thể thực hiện các bước như hình để tắt đi
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/31_WCI_ServerManager.png)
* Click **Local Server** -> **On** (IE Enhanced Security Configuration)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/32_WCI_IE_EnhancedOff.png)
* Click tùy chọn **Off** và chọn **OK** như trong hình
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/33_WCI_IE_EnhancedOff.png)
* Tiếp theo, mở **Windows Explorer** -> **This PC** -> Double Click **CD Drive (D:)** để truy cập ổ đĩa ảo của VirtIO drivers
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/34_WCI_OpenVirtIODrive.png)
* Double click vào file như trong hình để tiến hành cài đặt **Qemu Guest Agent**, các driver còn thiếu và các thành phần liên quan
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/35_WCI_InstallVirtIOGuestTools.png)
* Tick chọn ở **(1)** và bấm **Install**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/36_WCI_InstallVirtIOGuestTools_Step1.png)
* Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/37_WCI_InstallVirtIOGuestTools_Step2.png)
* Tick chọn ở **(1)** và bấm **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/38_WCI_InstallVirtIOGuestTools_Step3.png)
* Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/39_WCI_InstallVirtIOGuestTools_Step4.png)
* Click **Install**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/40_WCI_InstallVirtIOGuestTools_Step5.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/41_WCI_InstallVirtIOGuestTools_Step5_View.png)
* Click **Finish**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/42_WCI_InstallVirtIOGuestTools_Step6.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/43_WCI_InstallVirtIOGuestTools_Step6_View.png)
* Click **Close**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/44_WCI_InstallVirtIOGuestTools_Step7.png)
* Hiện tại thì VM Windows chưa có kết nối Internet. Ta cần cấu hình IP tĩnh cho VM
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/45_WCI_NoNetworkStatusVM.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/46_WCI_SetNetworkConfig.png)
* Tiếp đến, ta truy cập trình duyệt IE để tiến hành tải Cloudbase-Init
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/47_WCI_EnterIE.png)
* Truy cập [Cloudbase-init download](https://cloudbase.it/cloudbase-init/#download) và click chọn **Cloudbase-Init x64** như hình
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/48_WCI_ClickDownloadCI.png)
* Click **Run** để sau khi tải xong và khởi chạy ngay tiến trình cài đặt
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/49_WCI_ClickRUN.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/50_WCI_DownloadProgress.png)
* Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/51_WCI_CI_Step1.png)
* Tick chọn ở (1) và click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/52_WCI_CI_Step2.png)
* Click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/53_WCI_CI_Step3.png)
* Tick chọn ở (1) để kích hoạt Cloudbase-Init service chạy như dịch vụ hệ thống và click **Next**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/54_WCI_CI_Step4.png)
* Click **Install**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/55_WCI_CI_Step5.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/56_WCI_CI_Step5_View.png)
* Click **Finish**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/57_WCI_CI_Step6.png)
* Tiếp theo, ta truy cập thư mục **C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf**, sửa lại file **cloudbase-init.conf** theo nội dung sau:
  ```
  [DEFAULT]
  username=Administrator
  groups=Administrators
  inject_user_password=true
  first_logon_behaviour=no
  config_drive_raw_hhd=true
  config_drive_cdrom=true
  config_drive_vfat=true
  bsdtar_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\bsdtar.exe
  mtools_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\bin\
  verbose=true
  debug=true
  log_dir=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\log\
  log_file=cloudbase-init.log
  default_log_levels=comtypes=INFO,suds=INFO,iso8601=WARN,requests=WARN
  logging_serial_port_settings=
  mtu_use_dhcp_config=true
  ntp_use_dhcp_config=true
  local_scripts_path=C:\Program Files\Cloudbase Solutions\Cloudbase-Init\LocalScripts\
  check_latest_version=true
  metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService
  plugins=cloudbaseinit.plugins.common.networkconfig.NetworkConfigPlugin,cloudbaseinit.plugins.common.setuserpassword.SetUserPasswordPlugin
  ```
  > ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/58_WCI_CI_Step7.png)
  > * Tại phần **username**, ta sẽ đổi thành **Administrator** -> áp dụng các phần cấu hình password, network chỉ cho user này.
  > * Chèn thêm **first_logon_behaviour=no** để Windows buộc người dùng đặt lại password cho lần login đầu tiên.
  > * Thêm dòng **metadata_services=cloudbaseinit.metadata.services.configdrive.ConfigDriveService** để Cloudbase-Init đọc dữ liệu từ ổ đĩa ảo **CloudInit Drive** mà ta đã thêm ở bước trước.
  > * Tại phần **plugins**, ta sẽ thêm 2 plugins về cấu hình Network, Password; ở đây có thể bỏ phần plugins này thì mặc định Cloudbase-Init sẽ chạy tất cả các plugin mà có hỗ trợ cho Windows; nhưng vẫn nên thêm cụ thể các plugins mà ta cần dùng để phần cấu hình rõ ràng hơn và tránh các lỗi không cần thiết. Nếu metadata gửi từ Cloud-Init của proxmox có sự thay đổi thì sẽ thực thi các plugin theo thứ tự từ trái qua phải.
  > * Tham khảo thêm tại [Cloudbase-Init Docs](https://cloudbase-init.readthedocs.io/en/latest/tutorial.html#configuration-file)
* Tiếp theo, ta cần cấu hình dịch vụ **cloudbase-init** service -> đổi **Startup Type** sang **Automatic (Delayed Start)**
  * Mở cửa sổ **Run** dùng tổ hợp phím 'Windows' + R, nhập **services.msc** và bấm **OK**
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/60_WCI_AccessServicesList.png)
  * Click chuột phải vào dịch vụ **cloudbase-init** và chọn **Properties**
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/61_WCI_ServiceProperties.png)
  * Đổi **Startup type** sang **Automatic (Delayed Start)**
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/62_WCI_ChooseStartupTypeOption.png)
  * Click **OK** để lưu thiết lập
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/63_WCI_ClickOK.png)
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/64_WCI_Done.png)
  > Có một số trường hợp sau khi reboot lại thì không áp dụng metadata gửi từ Cloud-Init của Proxmox; nguyên nhân của sự cố này có thể là do sai lệch thứ tự boot của Cloudbase-Init service (Cloudbase-Init service có thể load quá sớm trước khi nhận dạng được metadata mount từ ổ đĩa ảo CloudInit Drive Cloud-Init Proxmox -> plugin không thực thi/ áp dụng metadata mới). Vì thế ta cần cấu hình lại **Startup Type** như trên để hoãn lại thời gian khởi chạy cloud-init service, đợi OS mount và nhận dạng ổ đĩa ảo CloudInit Drive.
* Kiểm thử áp dụng cấu hình từ Cloud-Init Proxmox
  * Cấu hình network của VM trước khi áp dụng cấu hình từ Cloud-Init Proxmox như hình
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/65_WCI_ConfigNetwork_View.png)
  * Tiếp đến, ta click tab **Cloud-Init** và tiến hành sửa các thông tin cấu hình như trong hình, sau đó click **Regenerate Image**
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/66_WCI_ConfigCloud-Init.png)
  * Sau đó, **Reboot** lại VM, và kiểm tra lại cấu hình mạng, mật khẩu
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/67_WCI_CheckNetworkConfig.png)
* Tiếp đến, ta sẽ chạy Sysprep để buộc xóa các thông tin nhận dạng phần cứng (Hardware Identity) mà OS nhận diện được -> buộc OS phải nhận dạng lại ở lần boot tiếp theo
  * Trước tiên, ta sẽ cài đặt Chrome trên VM
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/68_WCI_InstallChrome.png)
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/69_WCI_ChromeView.png)
  * Tiếp đến, mở cửa sổ CMD và truy cập đến đường dẫn *C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf* thông qua lệnh
    ```
    cd C:\Program Files\Cloudbase Solutions\Cloudbase-Init\conf
    ```
    và chạy tiếp lệnh sau để bắt đầu chạy Sysprep
    ```
    C:\Windows\System32\Sysprep\sysprep.exe /generalize /oobe /unattend:Unattend.xml
    ```
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/70_WCI_ExcuteCommand.png)
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/71_WCI_SysPrepLoading.png)
  * Sau khi chạy xong thì sẽ tự động shutdown VM
    ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/72_WCI_SysprepStopVM.png)
* Sau khi chạy sysprep thành công thì ta có thể chuyển đổi VM thành template - nhấp chuột phải vào VM và click chọn **Convert to template**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/73_WCI_ConvertToTemplate.png)
* Click **Yes**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/74_WCI_Yes.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/75_WCI_TemplateView.png)
* Clone thành VM - Nhấp chuột phải vào template (1) và click chọn **Clone** (2)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/76_WCI_CloneToVM.png)
* Điền tên VM vào trường **Name**, chọn **Mode** là **Full Clone** và bấm **Clone**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/77_WCI_InputVMInfo.png)
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/78_WCI_VMCloneView.png)
* Click chọn tab **Cloud-Init**, đổi/nhập lại password ở trường **Password** và bấm **Regenerate Image**
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/79_WCI_InputPasswordAndRegenrateImage.png)
* **Start** VM, nhập **Password** đã điền trong **Cloud-Init** ở bước trước để vào VM
  ![](../Proxmox/images/Cloud-Init/Windows_CloudbaseInit/80_WCI_VMConsoleView.png)
  
    
## Tạo template Windows tích hợp với Cloudbase-Init
> Coming soon
