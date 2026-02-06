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







