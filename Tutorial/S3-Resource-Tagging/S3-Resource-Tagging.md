# Hướng dẫn sử dụng tính năng Resource Tagging

## 1. Đăng ký sử dụng gói lưu trữ dữ liệu - S3
* Nếu quý khách chưa có gói S3, vui lòng tham khảo và đăng ký dịch vụ theo [link](https://vndata.vn/cloud-s3-object-storage-vietnam/).
  ![](../S3-Resource-Tagging/images/001.png)

## 2. Truy cập và cấu hình Resource Tagging
* Truy cập vào [VNDATA S3 Portal](https://cloud.vndata.vn/) và đăng nhập bằng thông tin tài khoản tương tự trang [VNDATA - Clients Portal](https://clients.vndata.vn/).
  ![](../S3-Resource-Tagging/images/002.png)
* Sau khi đăng nhập thành công, chọn mục **Object Storage** ở thanh menu bên trái.
  ![](../S3-Resource-Tagging/images/003.png)
* Click chọn vào gói S3 tương ứng cần thao tác.
  ![](../S3-Resource-Tagging/images/004.png)
* Click **Buckets**
  ![](../S3-Resource-Tagging/images/005.png)
  
### 2.1 Quản lí tag cho bucket
* Để gắn tag cho bucket ta thực hiện theo các bước sau đây:
  * Chọn dấu ```...``` -> **Details** -> **Tags**
    ![](../S3-Resource-Tagging/images/006.png)
    ![](../S3-Resource-Tagging/images/007.png)
  * Điền tag key và tag value lần lượt vào ô **KEY** và **VALUE**
  * *(Ở trong ví dụ này thì sẽ điền **KEY** là **Name** và **VALUE** là **Bucket02**)*
    ![](../S3-Resource-Tagging/images/008.png)
    
    > Lưu ý: có thể chỉ cần điền chuỗi vào ô **KEY**; ô **VALUE** có thể để trống (ứng với chuỗi rỗng)
  
  * Bấm **Save Tags** để lưu tag lại cho bucket
    ![](../S3-Resource-Tagging/images/009.png)
* Ví dụ: Chuyển dữ liệu bucket đã lọc theo bucket tag để chuyển sang tài khoản/endpoint S3 khác
  * Lọc theo mỗi **Tag KEY**
    ![](../S3-Resource-Tagging/images/010.png)
    ![](../S3-Resource-Tagging/images/011.png)
    ![](../S3-Resource-Tagging/images/012.png)
  * Lọc theo **Tag KEY** và **Tag VALUE**
    ![](../S3-Resource-Tagging/images/013.png)
    ![](../S3-Resource-Tagging/images/014.png)

  




