# Hướng dẫn sử dụng tính năng Resource Tagging

## 1. Đăng ký sử dụng gói lưu trữ dữ liệu - S3
* Nếu quý khách chưa có gói S3, vui lòng tham khảo và đăng ký dịch vụ theo [link](https://vndata.vn/cloud-s3-object-storage-vietnam/).
  ![](../S3-Resource-Tagging/images/001.png)

## 2. Truy cập và cấu hình Resource Tagging
* Truy cập vào [VNDATA S3 Portal](https://cloud.vndata.vn/) và đăng nhập bằng thông tin tài khoản (tương tự trang [VNDATA - Clients Portal](https://clients.vndata.vn/)).
  ![](../S3-Resource-Tagging/images/002.png)
* Sau khi đăng nhập thành công, chọn mục **Object Storage** ở thanh menu bên trái.
  ![](../S3-Resource-Tagging/images/003.png)
* Click chọn vào gói S3 tương ứng cần thao tác.
  ![](../S3-Resource-Tagging/images/004.png)
* Click vào tab **Buckets**.
  ![](../S3-Resource-Tagging/images/005.png)
  
### 2.1 Quản lý Tag cho Bucket
Để gắn Tag cho Bucket, quý khách thực hiện theo các bước sau:
* Tại Bucket cần thao tác, chọn biểu tượng `...` -> **Details** -> **Tags**.
  ![](../S3-Resource-Tagging/images/006.png)
  ![](../S3-Resource-Tagging/images/007.png)
* Điền Tag Key và Tag Value lần lượt vào các ô tương ứng.
* *(Trong ví dụ này, chúng ta sẽ điền **KEY** là `Name` và **VALUE** là `Bucket02`)*.
  ![](../S3-Resource-Tagging/images/008.png)
  
  > **Lưu ý:** Quý khách có thể chỉ cần điền chuỗi vào ô **KEY**. Ô **VALUE** có thể để trống (tương đương với chuỗi rỗng).
  
* Bấm **Save Tags** để lưu cấu hình.
  ![](../S3-Resource-Tagging/images/009.png)

**Ứng dụng thực tế: Dùng Bucket Tag để tự động hóa chuyển vùng dữ liệu (Data Transfer)**
> Thay vì thao tác thủ công, quý khách có thể kết hợp Tag với các công cụ (như AWS CLI và Rclone) để lọc và chuyển dữ liệu đồng loạt sang tài khoản/endpoint S3 khác.
* Lọc Bucket chỉ theo **Tag KEY**:
  ![](../S3-Resource-Tagging/images/010.png)
  ![](../S3-Resource-Tagging/images/011.png)
  ![](../S3-Resource-Tagging/images/012.png)
* Lọc Bucket theo cả **Tag KEY** và **Tag VALUE**:
  ![](../S3-Resource-Tagging/images/013.png)
  ![](../S3-Resource-Tagging/images/014.png)

### 2.2 Quản lý Tag cho Object (File)
Để gắn Tag cho từng File cụ thể, quý khách thực hiện như sau:
* Truy cập vào Bucket chứa file cần gắn Tag.
  ![](../S3-Resource-Tagging/images/015.png)
* Tại file mục tiêu, chọn **Details** -> **Tags**.
  ![](../S3-Resource-Tagging/images/016.png)
  ![](../S3-Resource-Tagging/images/017.png)
* Điền thông tin vào ô **KEY** và **VALUE**.
* *(Trong ví dụ này, chúng ta điền **KEY** là `File` và **VALUE** là `TEXT`)*.
  ![](../S3-Resource-Tagging/images/018.png)

  > **Lưu ý:** Tương tự như Bucket Tag, quý khách có thể bỏ trống ô **VALUE** nếu chỉ cần dùng **KEY** để phân loại.

* Bấm **Save Tags** để hoàn tất.
  ![](../S3-Resource-Tagging/images/019.png)

**Ứng dụng thực tế: Kết hợp Object Tag với Lifecycle Rules**
Tính năng quản lý vòng đời (Lifecycle) cho phép tự động dọn dẹp hoặc chuyển đổi dữ liệu dựa trên Tag của File.
> Quý khách có thể tìm hiểu chi tiết về tính năng này qua bài viết: [Hướng dẫn sử dụng S3 Lifecycle Rules](https://wiki.vndata.vn/s3-object-storage/huong-dan-su-dung/s3-bucket-versioning/).

* **Kịch bản:** Gắn Tag cho các file `001.txt, 002.txt, 003.txt, 004.txt` với thông tin **KEY:VALUE** là `status:delete`.
* Thiết lập **Lifecycle Rule** cho các file trên vào các Tag này như hình dưới:
  ![](../S3-Resource-Tagging/images/020.png)
* **Kết quả:** Hệ thống sẽ tự động xử lý các file khớp điều kiện.
  ![](../S3-Resource-Tagging/images/021.png)

  > **Lưu ý:** Để Rule áp dụng chính xác lên một File, cấu hình trong Rule phải **khớp hoàn toàn** cả Tag Key và Tag Value của File đó. 
  > *Ví dụ: Nếu Rule chỉ quy định `status:`, nó sẽ bỏ qua file `003.txt` (có tag là `status:delete`).*
  ![](../S3-Resource-Tagging/images/022.png)



