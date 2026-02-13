# Hướng dẫn sử dụng tính năng Bucket Versioning và Lifecycle Rules

## 1. Đăng kí sử dụng gói lưu trữ dữ liệu - S3
* Nếu quý khách chưa có gói S3 thì tham khảo và đăng kí dịch vụ theo [link](https://vndata.vn/cloud-s3-object-storage-vietnam/).
  ![](../S3-Bucket-Versioning/images/001.png)

## 2. Sau khi đăng kí gói S3 thành công (Hoặc quý khách đã có gói S3)
* Truy cập vào [VNDATA S3 Portal](https://cloud.vndata.vn/) và login tài khoản với các thông tin (Địa chỉ email/ mật khẩu) tương tự trang [VNDATA - Clients Portal](https://clients.vndata.vn/)
  ![](../S3-Bucket-Versioning/images/002.png)
* Sau khi vào được trang S3 Portal -> click chọn **Object Storage**
  ![](../S3-Bucket-Versioning/images/003.png)
* Click chọn vào gói S3 tương ứng cần cấu hình **Bucket Versioning**
  ![](../S3-Bucket-Versioning/images/004.png)
* Chọn **Buckets** -> **Details** tại bucket cần cấu hình **Bucket Versioning**
  ![](../S3-Bucket-Versioning/images/005.png)
* Click nút gạt và nhấn **Submit** như trong hình để bật tính năng **Bucket Versioning**
  ![](../S3-Bucket-Versioning/images/006.png)
  > **Bucket Versioning** là tính năng giúp lưu giữ lại các phiên bản cũ của file để có thể restore lại khi cần thiết thay vì file bị xóa khi upload một file cùng tên (hoặc xóa nhầm file).
* Tại vì bucket hiện tại chưa có dữ liệu nên phần **Versions** đang trống
  ![](../S3-Bucket-Versioning/images/007.png)
* Tiến hành upload một file text (**test.txt**) với nội dung **abcdefgh**
  ![](../S3-Bucket-Versioning/images/008.png)
  ![](../S3-Bucket-Versioning/images/009.png)
* Từ máy local, sửa file **test.txt** với nội dung **12345678** và upload lên lại chính bucket đó
  ![](../S3-Bucket-Versioning/images/010.png)
  ![](../S3-Bucket-Versioning/images/011.png)
* Khi đó file **test.txt** cũ sẽ bị ẩn đi (trở thành phiên bản cũ)
  ![](../S3-Bucket-Versioning/images/012.png)
* Tại đây, ta có thể restore file **test.txt** ở bản cũ (**Previous**)
  ![](../S3-Bucket-Versioning/images/013.png)
* Khi đó, sẽ tạo ra một bản sao của chính version cũ đó và đặt thành **Current**
  ![](../S3-Bucket-Versioning/images/014.png)
  ![](../S3-Bucket-Versioning/images/015.png)
* Khi xóa file thì sẽ tạo một delete marker trong Versions và ẩn file khỏi giao diện **Objects**
  ![](../S3-Bucket-Versioning/images/016.png)
  ![](../S3-Bucket-Versioning/images/017.png)
  ![](../S3-Bucket-Versioning/images/018.png)
  > Tuy nhiên, việc lưu trữ mọi phiên bản của file cũng có nhược điểm. Nếu không được kiểm soát, các phiên bản cũ **(non-current versions)** sẽ tích tụ theo thời gian, chiếm dụng lớn dung lượng bucket và làm tăng chi phí cho những dữ liệu không còn giá trị sử dụng. Để khắc phục, **Lifecycle Rule** là giải pháp giúp tự động hóa việc quản lý vòng đời của file (tự động xóa hoặc chuyển sang **Cold Storage** để tối ưu chi phí).
* Để truy cập vào tính năng **Lifecycle Rule** thì tại giao diện **Buckets**, click chọn **Lifecycle configuration** vào bucket cần cấu hình
  ![](../S3-Bucket-Versioning/images/019.png)
* Click chọn **Add Lifecycle Rule** để mở giao diện cấu hình
  ![](../S3-Bucket-Versioning/images/020.png)
  ![](../S3-Bucket-Versioning/images/021.png)
  * Sơ lược về các thông số trên:
    * **Rule ID**: ở trường này thì điền ID theo tên gợi nhớ để dễ quản lí hoặc để trống để hệ thống tự điền.
      ![](../S3-Bucket-Versioning/images/022.png)
    * **Status**: bao gồm **Enabled/Disabled** - kích hoạt/vô hiệu hóa rule.
    * **Prefix Filter**: trường này ta sẽ định nghĩa các file/fake folder hoặc tất cả các dữ liệu trong bucket được áp dụng bởi rule.
      * Ví dụ:
        * Áp dụng với tất cả dữ liệu trong bucket: để trống trường này.
        * Áp dụng với việc xóa tất cả dữ liệu trong thư mục logs/: điền **logs/**
        * Áp dụng với việc xóa tất cả dữ liệu trong thư mục month_backups/ lồng trong thư mục backups/: điền **backups/month_backups/**
        * Áp dụng với việc xóa dữ liệu thuộc các file và fake folder bắt đầu bằng chuỗi **logs**: điền **logs**
        * Áp dụng với việc xóa đúng tên file **abc123.txt**: điền **abc123.txt**
    * **Transition to another storage class after (days)**:
    * **Storage Class**:
    * **Expire (delete) after (days)**: Nhập số ngày tồn tại của file hiện hành **(Current Version)**. Khi hết hạn, hệ thống sẽ gán **Delete Marker** cho file đó (file sẽ ẩn đi trong giao diện chính nhưng vẫn còn trong bucket).
    * **Expire noncurrent versions after (days)**: Nhập số ngày lưu trữ các phiên bản cũ **(Noncurrent Versions)**. Thời gian bắt đầu tính từ lúc phiên bản đó trở thành cũ (do bị ghi đè bởi file với phiên bản mới hoặc delete marker). Sau thời gian này, chúng sẽ bị xóa vĩnh viễn để giải phóng dung lượng.
    > Cách tính ngày ở 3 trường trên sẽ như sau: ví dụ với file được upload vào mọi khung giờ từ 00:01 -> 23:59 ngày 10/02 và thiết đặt Expire days là 3 ngày thì file sẽ hết hạn vào 0h UTC+0 (tức 7h sáng) vào 14/2.
* Các ví dụ thực tế:
  * Xóa file với tên file bắt đầu bằng chuỗi **Expire-delete** sau khi hết hạn 1 ngày
    ![](../S3-Bucket-Versioning/images/023.png)
    ![](../S3-Bucket-Versioning/images/024.png)








