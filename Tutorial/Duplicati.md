# Hướng dẫn tích hợp Duplicati với S3 Storage dùng cho Backup
## Trên Windows
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
* Tiếp đến, sẽ tự động chuyển tiếp qua trang đăng nhập nếu chưa đăng kí/đăng nhập tài khoản
  ![](../Tutorial/images/14_Duplicati_SignIn.png)
* Có thể điền tên VM tại **Machine name** hoặc để mặc định và bấm **Register machine**
  ![](../Tutorial/images/15_Duplicati_RegisterMachine.png)
  ![](../Tutorial/images/16_Duplicati_BacktoPortal.png)
* Nếu lần đầu Đăng kí/Đăng nhập tài khoản **Duplicati** thì sẽ phải thực hiện thêm bước này (Chọn **organization type** phù hợp; click check ở (2), (3) -> click **Save**)
  ![](../Tutorial/images/17_Duplicati_SaveInfo.png)
  ![](../Tutorial/images/18_Duplicati_Dashboard.png)
* Click **Machines** -> **Details** -> **Start Session** để mở giao diện của **Duplicati** server trên Windows VM

* 
  

    
URL import: s3://duplicati/window01?s3-server-name=s3-hcm-r2.s3cloud.vn&auth-username=<>&auth-password=<>&use-ssl=true

> So sánh snapshot-policy=required 

Ngoại lệ:
Sau khi khởi động lại Window VM thì cần Login Administrator để Duplicati có thể chạy backup theo lịch

## Trên Linux Server
> Coming soon

