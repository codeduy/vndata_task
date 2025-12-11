# Hướng dẫn tích hợp Duplicati với Cloud Object Storage S3 tại VNData dùng cho Sao lưu - Phục hồi dữ liệu
## Trên Windows Server
* Đầu tiên, ta cần cài **Duplicati** trên Window qua đường link [Duplicati Download](https://duplicati.com/download)
  * Sau khi truy cập vào đường link trên, click chọn **Download for Windows**
    ![](../Tutorial/images/1_Duplicati_DownloadforWindows.png)
  * Sau khi tải xong thì tiến hành chạy file cài đặt -> Click **Next**
    ![](../Tutorial/images/2_Duplicati_clickNext.png)
  * Click **I accept the terms in the License Agreement** -> Click **Next**
    ![](../Tutorial/images/3_Duplicati_ClickAcceptandNext.png)
  * Click **Next**
    ![](../Tutorial/images/4_Duplicati_ClickNext.png)
  * Click **Install**
    ![](../Tutorial/images/5_Duplicati_clickInstall.png)
    ![](../Tutorial/images/6_Duplicati_InstallWaiting.png)
  * Click **Finish**
    ![](../Tutorial/images/7_Duplicati_finishClick.png)
* Tiếp theo, mở **Duplicati** qua các thao tác click vào dấu mũi tên **^** (Show hidden icons) (1) để mở rộng khu vực System Tray (Khay hệ thống) -> click chuột phải vào **Tray icon** (biểu tượng) của ứng dụng **Duplicati** (2) -> Click chọn **Open** (3) -> Mở giao diện tương tác của **Duplicati** server
  ![](../Tutorial/images/8_Duplicati_Open.png)
* Nếu có nhu cầu đặt **passphrase** thì có thể đặt tại bước này hoặc ta có thể đặt sau. Trong phạm vi bài hướng dẫn này thì không cần thiết phải đặt **passphrase** -> click **Not now thanks**
  ![](../Tutorial/images/9_Duplicati_NotNow.png)
* Click **Settings** -> **Register on console** để lấy liên kết - kết nối Windows VM này với giao diện quản lí tập trung của **Duplicati**
  ![](../Tutorial/images/10_Duplicati_RegisterOnConsole.png)
  ![](../Tutorial/images/11_Duplicati_RegistrationWaiting.png)
* Sau khi **Register on console** thành công thì sẽ hiện ra liên kết như hình, ta copy link này
  ![](../Tutorial/images/12_Duplicati_ReleaseMachineLink.png)
* Dán và truy cập đường link trên vào trình duyệt
  ![](../Tutorial/images/13_Duplicati_PasteLink.png)
* Tiếp đến, sẽ tự động chuyển tiếp qua trang đăng nhập nếu chưa đăng kí/đăng nhập tài khoản. Cần đăng kí/đăng nhập tài khoản **Duplicati** để tiếp tục đến bước tiếp theo
  ![](../Tutorial/images/14_Duplicati_SignIn.png)
* Có thể điền tên VM tại **Machine name** hoặc để mặc định và bấm **Register machine**
  ![](../Tutorial/images/15_Duplicati_RegisterMachine.png)
  ![](../Tutorial/images/16_Duplicati_BacktoPortal.png)
* Nếu lần đầu Đăng kí/Đăng nhập tài khoản **Duplicati** thì sẽ phải thực hiện thêm bước này (Chọn **organization type** phù hợp; click check ở (2), (3) -> click **Save**)
  ![](../Tutorial/images/17_Duplicati_SaveInfo.png)
  ![](../Tutorial/images/18_Duplicati_Dashboard.png)
* Click **Machines** -> **Details** -> **Start Session** để mở giao diện của **Duplicati** server trên Windows VM
  ![](../Tutorial/images/19_Duplicati_StartSession.png)
* Click **Create** tại **Backups**
  ![](../Tutorial/images/20_Duplicati_CreateClick.png)
* Click **Add a new backup**
  ![](../Tutorial/images/21_Duplicati_AddNewBackUp.png)
* Điền tên của job backup vào **Backup name**, điền mật khẩu vào **Create a password** để tạo mật khẩu dùng cho mã hóa và giải mã dữ liệu lưu trữ ở **Destination**
  ![](../Tutorial/images/22_Duplicati_General.png)
* Sau khi điền xong thì click chọn **Continue**
  ![](../Tutorial/images/23_Duplicati_General_Continue.png)
* Tại phần **Destination** -> chọn **S3 Compatible**
  ![](../Tutorial/images/24_Duplicate_S3Choosing.png)
* Nếu quý khách chưa có gói **Cloud Object Storage S3** để dùng cho việc backup thì có thể tham khảo các gói lưu trữ của [**VNData**](https://vndata.vn/) qua đường dẫn [**VNData - Cloud Object Storage S3**](https://vndata.vn/cloud-s3-object-storage-vietnam/)  
  ![](../Tutorial/images/25-1_Duplicate_VNDataS3Service.png)
* Điền các thông tin tương ứng vào **Bucket Name** - tên bucket S3, **Folder path** - đường dẫn folder trong bucket đó, **Server** - điền thông tin tại phần **S3 ENDPOINT (HTTPS)** (bỏ phần "https"); tick chọn **Instruct Duplicati to use an SSL (https) connection**; điền lần lượt **Access Key Id**, **Secret Key** ở trang **S3 Portal - VNData** vào **AWS Access Key ID** và **AWS Secret Access Key**
  ![](../Tutorial/images/25-2_Duplicate_DesView.png)
  ![](../Tutorial/images/25-3_Duplicate_DesView.png)
* Trong bài hướng dẫn này thì mình đã tạo sẵn **S3 Bucket** với tên là **duplicati** và thư mục là **windows-demo**
  ![](../Tutorial/images/26_Duplicati_S3View.png)
  ![](../Tutorial/images/27_Duplicati_S3PortalView.png)
  ![](../Tutorial/images/28_Duplicati_DesView.png)
  ![](../Tutorial/images/29_Duplicati_DesView.png)
* Sau khi điền xong thì click **Test destination** để kiểm tra kết nối đến Bucket S3 với các thông tin đã điền ở trên
  ![](../Tutorial/images/30_Duplicati_TestConnection.png)
  ![](../Tutorial/images/31_Duplicati_TestConn.png)
* Sau khi kiểm tra thành công thì click **Continue**
  ![](../Tutorial/images/32-1_Duplicati_Continue.png)
* Tại phần **Source Data**, ta có thể chọn nhiều file/thư mục để backup. Sau khi chọn xong thì bấm **Continue**
  ![](../Tutorial/images/32_Duplicati_SoureData.png)
* Đến mục **Schedule** - đặt lịch sao lưu, tùy nhu cầu/mục đích quản trị mà sẽ tùy chỉnh lịch sao lưu phù hợp. Sau khi chỉnh xong thì click **Continue**
  ![](../Tutorial/images/33_Duplicati_Schedule.png)  
* Tại mục **Options**; có thể tùy chỉnh thêm tại **Remote volume size**, **Backup retention** và click **Submit**
  ![](../Tutorial/images/35_Duplicati_Options.png)
  ![](../Tutorial/images/36_Duplicati_Dashboard.png)
* Click **Start** để kiểm thử việc sao lưu dữ liệu lên **Cloud Object Storage S3**
  ![](../Tutorial/images/37_Duplicati_StartBackup.png)
  ![](../Tutorial/images/38_Duplicati_BackupLoading.png)
  ![](../Tutorial/images/39_Duplicati_BackupLoading.png)
  ![](../Tutorial/images/40_Duplicati_BackupSuccess.png)
* Như trong hình thì dữ liệu đã được backup và lưu thành công lên **Object Storage**
  ![](../Tutorial/images/41_Duplicati_BucketView.png)
* Để **Restore** - Phục hồi dữ liệu từ bản Backup thì click **Start** tại mục **Restores**
  ![](../Tutorial/images/42_Duplicati_RestoreClick.png)
* Tại đây, sẽ hiện bản backup với tên job backup tương ứng mà ta đã tạo từ trước đó -> bấm nút **Restore** với job backup mà bạn muốn phục hồi
  ![](../Tutorial/images/43_Duplicati_RestoreBackupJob.png)
* Chọn file/thư mục cần phục hồi và bấm **Continue**
  ![](../Tutorial/images/44_Duplicati_File-FolderSelect.png)
* Tại phần **Restore Options**, tùy chỉnh các tùy chọn theo nhu cầu và bấm **Submit**
  ![](../Tutorial/images/45_Duplicati_RestoreOptions.png)
  ![](../Tutorial/images/46_Duplicati_SubmitClick.png)
  ![](../Tutorial/images/47_Duplicati_RestoreinProgress.png)
  ![](../Tutorial/images/48_Duplicati_RestoreSuccessful.png)
  
> Lưu ý:
> * Phần hướng dẫn trên thực hiện trên **Window Server** 2019 với user mặc định là **Administrator**
> * Sau khi khởi động lại Window VM thì cần Login VM lại với user **Administrator** để **Duplicati** có thể chạy backup theo lịch(nếu có)



Với mode LocalSystem - Window service 

-> start session, auto backup schedule kể cả user log out

## Trên Linux Server
> Coming soon




