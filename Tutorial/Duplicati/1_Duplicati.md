# Hướng dẫn tích hợp Duplicati với Cloud Object Storage S3 tại VNData dùng cho Sao lưu - Phục hồi dữ liệu
## Trên Windows Server
### Cài đặt Duplicati ở dạng Foreground App
* Đầu tiên, ta cần cài **Duplicati** trên Window qua đường link [Duplicati Download](https://duplicati.com/download)
  * Sau khi truy cập vào đường link trên, click chọn **Download for Windows**
    ![](../Duplicati/images/1_Duplicati_DownloadforWindows.png)
  * Sau khi tải xong thì tiến hành chạy file cài đặt
    ![](../Duplicati/images/2_Duplicati_clickNext.png)
  * Click **Finish**
    ![](../Duplicati/images/7_Duplicati_finishClick.png)
* Tiếp theo, mở **Duplicati** qua **Tray icon** của ứng dụng **Duplicati**
  ![](../Duplicati/images/8_Duplicati_Open.png)
* Có thể đặt **passphrase** ở bước này hoặc để sau
  ![](../Duplicati/images/9_Duplicati_NotNow.png)
* Click **Settings** -> **Register on console** để cấu hình remote session
  ![](../Duplicati/images/10_Duplicati_RegisterOnConsole.png)
  ![](../Duplicati/images/12_Duplicati_ReleaseMachineLink.png)
* Truy cập đường link trên và Đăng kí/Đăng nhập tài khoản **Duplicati**
  ![](../Duplicati/images/14_Duplicati_SignIn.png)
* Click **Register machine**
  ![](../Duplicati/images/15_Duplicati_RegisterMachine.png)
  ![](../Duplicati/images/18_Duplicati_Dashboard.png)
* Click **Machines** -> **Details** -> **Start Session** để mở giao diện của **Duplicati** server trên Windows VM
  ![](../Duplicati/images/19_Duplicati_StartSession.png)
* Click **Create** tại **Backups**
  ![](../Duplicati/images/20_Duplicati_CreateClick.png)
* Click **Add a new backup**
  ![](../Duplicati/images/21_Duplicati_AddNewBackUp.png)
* Điền tên của job backup vào **Backup name**, điền mật khẩu vào **Create a password** và click **Continue**
  ![](../Duplicati/images/22_Duplicati_General.png)
  ![](../Duplicati/images/23_Duplicati_General_Continue.png)
* Tại phần **Destination** -> chọn **S3 Compatible**
  ![](../Duplicati/images/24_Duplicate_S3Choosing.png)
* Nếu quý khách chưa có gói **Cloud Object Storage S3** để dùng cho việc backup thì có thể tham khảo các gói lưu trữ của [**VNData**](https://vndata.vn/) qua đường dẫn [**VNData - Cloud Object Storage S3**](https://vndata.vn/cloud-s3-object-storage-vietnam/)  
  ![](../Duplicati/images/25-1_Duplicate_VNDataS3Service.png)
* Điền các thông tin tương ứng vào **Bucket Name** - tên bucket S3, **Folder path** - đường dẫn folder trong bucket đó, **Server** - điền thông tin tại phần **S3 ENDPOINT (HTTPS)** (bỏ phần "https"); tick chọn **Instruct Duplicati to use an SSL (https) connection**; điền lần lượt **Access Key Id**, **Secret Key** ở trang **S3 Portal - VNData** vào **AWS Access Key ID** và **AWS Secret Access Key**
  ![](../Duplicati/images/28_Duplicati_DesView.png)
  ![](../Duplicati/images/29_Duplicati_DesView.png)
* Sau khi điền xong thì click **Test destination**. Trả kết quả thành công thì bấm **Continue**
  ![](../Duplicati/images/30_Duplicati_TestConnection.png)
  ![](../Duplicati/images/31_Duplicati_TestConn.png)
  ![](../Duplicati/images/32-1_Duplicati_Continue.png)
* Tại phần **Source Data**, chọn các file/thư mục cần sao lưu -> **Continue**
  ![](../Duplicati/images/32_Duplicati_SoureData.png)
* Đặt lịch backup tại **Schedule** -> **Continue**
  ![](../Duplicati/images/33_Duplicati_Schedule.png)  
* Tại mục **Options**; có thể tùy chỉnh thêm tại **Remote volume size**, **Backup retention** -> **Submit**
  ![](../Duplicati/images/35_Duplicati_Options.png)
  ![](../Duplicati/images/36_Duplicati_Dashboard.png)
* Click **Start** để kiểm thử việc sao lưu dữ liệu lên **Cloud Object Storage S3**
  ![](../Duplicati/images/37_Duplicati_StartBackup.png)
  ![](../Duplicati/images/40_Duplicati_BackupSuccess.png)
* Như trong hình thì dữ liệu đã được backup và lưu thành công lên **Object Storage**
  ![](../Duplicati/images/41_Duplicati_BucketView.png)
* Để **Restore** dữ liệu -> click **Start** tại mục **Restores**
  ![](../Duplicati/images/42_Duplicati_RestoreClick.png)
* Bấm **Restore** với bản sao lưu mà bạn muốn phục hồi
  ![](../Duplicati/images/43_Duplicati_RestoreBackupJob.png)
* Chọn file/thư mục cần **Restore** và bấm **Continue** -> **Submit**
  ![](../Duplicati/images/44_Duplicati_File-FolderSelect.png)
  ![](../Duplicati/images/46_Duplicati_SubmitClick.png)
  ![](../Duplicati/images/48_Duplicati_RestoreSuccessful.png)

### Cài đặt Duplicati ở dạng WindowsService
Với mode LocalSystem - Window service 

-> start session, auto backup schedule kể cả user log out


## Trên Linux Server
> Coming soon








