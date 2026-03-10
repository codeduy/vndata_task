# Hướng dẫn sử dụng tính năng Pre-signed URL

## 1. Đăng ký sử dụng gói lưu trữ dữ liệu - S3
* Nếu quý khách chưa có gói S3 thì tham khảo và đăng ký dịch vụ theo [link](https://vndata.vn/cloud-s3-object-storage-vietnam/).
  ![](../S3-Pre-signed-URL/images/001.png)

## 2. Sau khi đăng ký gói S3 thành công (Hoặc quý khách đã có gói S3)
* Truy cập vào [VNDATA S3 Portal](https://cloud.vndata.vn/) và đăng nhập tài khoản với các thông tin (Địa chỉ email/ mật khẩu) tương tự trang [VNDATA - Clients Portal](https://clients.vndata.vn/).
  ![](../S3-Pre-signed-URL/images/002.png)
* Sau khi vào được trang S3 Portal -> click chọn **Object Storage**.
  ![](../S3-Pre-signed-URL/images/003.png)
* Click chọn vào gói S3 tương ứng cần cấu hình **Pre-signed URL**.
  ![](../S3-Pre-signed-URL/images/004.png)
* Click chọn vào bucket chứa các object cần cấu hình **Pre-signed URL**.
  ![](../S3-Pre-signed-URL/images/005.png)
* Trong bucket đã upload sẵn file **s3_test.txt** với nội dung **abcxyz**
  ![](../S3-Pre-signed-URL/images/006.png)
  ![](../S3-Pre-signed-URL/images/007.png)
* Click chọn dấu ba chấm -> **Presign URL**
  ![](../S3-Pre-signed-URL/images/008.png)
* Tại đây, ta sẽ cần chú ý ở hai phần là **Method** và **Expiration**
  * **Method**: bao gồm ba phương thức HTTP là **GET**, **PUT**, **DELETE**
  * **Expiration**: bao gồm **No expiration (max 7d)**, **Expire by minutes**, **Expire by days**
    > Tại mục **Expiration**, ở 3 tùy chọn trên đều chỉ có thời gian quy đổi tối đa là 7 ngày
  
  ![](../S3-Pre-signed-URL/images/009.png)

## 3. Ví dụ thực tế
* **Ví dụ 01**: Thiết đặt **Presigned URL** với object **s3_test.txt**
  ![](../S3-Pre-signed-URL/images/010.png)
  * Các bước thao tác:
    * Ở phần **method** chọn **GET**, **Expiration** là 60 phút
    * Bấm chọn **Generate** -> click **Copy URL**
    * Kiểm thử **HTTP GET** qua lệnh
      ```
      curl -o s3_test.txt "Presigned URL đã generate ở bước trên"
      ```
      ![](../S3-Pre-signed-URL/images/011.png)
      
  
