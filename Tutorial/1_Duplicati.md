# Hướng dẫn tích hợp Duplicati với Cloud Object Storage S3 tại VNData dùng cho Sao lưu - Phục hồi dữ liệu
## Trên Windows Server
### Cài đặt Duplicati ở dạng Foreground App
* Đầu tiên, ta cần cài **Duplicati** trên Window qua đường link [Duplicati Download](https://duplicati.com/download)
  * Sau khi truy cập vào đường link trên, click chọn **Download for Windows**
    ![](../Tutorial/images/1_Duplicati_DownloadforWindows.png)
  * Sau khi tải xong thì tiến hành chạy file cài đặt
    ![](../Tutorial/images/2_Duplicati_clickNext.png)
  * Click **Finish**
    ![](../Tutorial/images/7_Duplicati_finishClick.png)
* Tiếp theo, mở **Duplicati** qua **Tray icon** của ứng dụng **Duplicati**
  ![](../Tutorial/images/8_Duplicati_Open.png)
* Có thể đặt **passphrase** ở bước này hoặc để sau
  ![](../Tutorial/images/9_Duplicati_NotNow.png)
* Click **Settings** -> **Register on console** để cấu hình remote session
  ![](../Tutorial/images/10_Duplicati_RegisterOnConsole.png)
  ![](../Tutorial/images/12_Duplicati_ReleaseMachineLink.png)
* Truy cập đường link trên và Đăng kí/Đăng nhập tài khoản **Duplicati**
  ![](../Tutorial/images/14_Duplicati_SignIn.png)
* Click **Register machine**
  ![](../Tutorial/images/15_Duplicati_RegisterMachine.png)
  ![](../Tutorial/images/18_Duplicati_Dashboard.png)
* Click **Machines** -> **Details** -> **Start Session** để mở giao diện của **Duplicati** server trên Windows VM
  ![](../Tutorial/images/19_Duplicati_StartSession.png)
* Click **Create** tại **Backups**
  ![](../Tutorial/images/20_Duplicati_CreateClick.png)
* Click **Add a new backup**
  ![](../Tutorial/images/21_Duplicati_AddNewBackUp.png)
* Điền tên của job backup vào **Backup name**, điền mật khẩu vào **Create a password** và click **Continue**
  ![](../Tutorial/images/22_Duplicati_General.png)
  ![](../Tutorial/images/23_Duplicati_General_Continue.png)
* Tại phần **Destination** -> chọn **S3 Compatible**
  ![](../Tutorial/images/24_Duplicate_S3Choosing.png)
* Nếu quý khách chưa có gói **Cloud Object Storage S3** để dùng cho việc backup thì có thể tham khảo các gói lưu trữ của [**VNData**](https://vndata.vn/) qua đường dẫn [**VNData - Cloud Object Storage S3**](https://vndata.vn/cloud-s3-object-storage-vietnam/)  
  ![](../Tutorial/images/25-1_Duplicate_VNDataS3Service.png)
* Điền các thông tin tương ứng vào **Bucket Name** - tên bucket S3, **Folder path** - đường dẫn folder trong bucket đó, **Server** - điền thông tin tại phần **S3 ENDPOINT (HTTPS)** (bỏ phần "https"); tick chọn **Instruct Duplicati to use an SSL (https) connection**; điền lần lượt **Access Key Id**, **Secret Key** ở trang **S3 Portal - VNData** vào **AWS Access Key ID** và **AWS Secret Access Key**
  ![](../Tutorial/images/28_Duplicati_DesView.png)
  ![](../Tutorial/images/29_Duplicati_DesView.png)
* Sau khi điền xong thì click **Test destination**. Trả kết quả thành công thì bấm **Continue**
  ![](../Tutorial/images/30_Duplicati_TestConnection.png)
  ![](../Tutorial/images/31_Duplicati_TestConn.png)
  ![](../Tutorial/images/32-1_Duplicati_Continue.png)
* Tại phần **Source Data**, chọn các file/thư mục cần sao lưu -> **Continue**
  ![](../Tutorial/images/32_Duplicati_SoureData.png)
* Đặt lịch backup tại **Schedule** -> **Continue**
  ![](../Tutorial/images/33_Duplicati_Schedule.png)  
* Click **Submit**
  ![](../Tutorial/images/35_Duplicati_Options.png)
  ![](../Tutorial/images/36_Duplicati_Dashboard.png)
* Click **Start** để chạy backup
  ![](../Tutorial/images/37_Duplicati_StartBackup.png)
  ![](../Tutorial/images/40_Duplicati_BackupSuccess.png)
* Như trong hình thì dữ liệu đã được backup và lưu thành công lên **Object Storage**
  ![](../Tutorial/images/41_Duplicati_BucketView.png)
* Để **Restore** dữ liệu -> click **Start** tại mục **Restores**
  ![](../Tutorial/images/42_Duplicati_RestoreClick.png)
* Bấm **Restore** với bản sao lưu mà bạn muốn phục hồi
  ![](../Tutorial/images/43_Duplicati_RestoreBackupJob.png)
* Chọn file/thư mục cần **Restore** và bấm **Continue** -> **Submit**
  ![](../Tutorial/images/44_Duplicati_File-FolderSelect.png)
  ![](../Tutorial/images/46_Duplicati_SubmitClick.png)
  ![](../Tutorial/images/48_Duplicati_RestoreSuccessful.png)

### Cài đặt Duplicati ở dạng WindowsService
* Tải và chạy file cài đặt **Duplicati**. Đến bước cuối cùng thì bỏ check **Launch Duplicati now** -> **Finish**
  ![](../Tutorial/images/49_Duplicati_Uncheck%26Finish.png)
* Mở **CMD** với quyền **Administrator** và chạy lần lượt các lệnh sau
  ```
  cd "C:\Program Files\Duplicati 2"
  ```
  ```
  Duplicati.WindowsService.exe INSTALL
  ```
  ![](../Tutorial/images/50_Duplicati_InstallService.png)
* Mở **Properties** của **Duplicati shortcut** ở màn hình Desktop và điền chuỗi sau vào trường **Target**:
  ```
  "C:\Program Files\Duplicati 2\Duplicati.GUI.TrayIcon.exe" --no-hosted-server --read-config-from-db --server-datafolder="C:\ProgramData\Duplicati"
  ```
  Và click **OK**
  ![](../Tutorial/images/51_Duplicati_PropertiesClick.png)
  ![](../Tutorial/images/52_Duplicati_TargetModify.png)
* Truy cập thư mục **C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp** hoặc chạy **Run** và thực thi lệnh **shell:common startup**. Điều chỉnh **Properties** của **Duplicati shortcut** tương tự bước trước
  ![](../Tutorial/images/53_Duplicati_TargetModify2.png)
* Hoàn thành các bước trên thì ta có thể truy cập giao diện **Duplicati** và sử dụng các tính năng **Backup**, **Restore** như bình thường
  ![](../Tutorial/images/54_Duplicati_PortalView.png)








