# Hướng dẫn triển khai K8s Cluster
## 1. Khởi tạo K8s Cluster
* Tại phần **Virtual Machines** -> chọn **Kubernetes** -> chọn **Create New**
  ![01](../K8s/images/01.png)
* Chọn phiên bản **Kubernetes**, Private **Network** (nếu không cần dùng chung mạng cục bộ với K8s Cluser khác thì có thể để trống phần này), cấu hình của mỗi node trong cluster tại **Choose Cluster Capacity** và số lượng worker node tại **Node Count**
  ![02](../K8s/images/02.png)
* Chọn dung lượng disk tương ứng cho từng node trong cluster
  ![03](../K8s/images/03.png)
* Ở tùy chọn **Enable High Availability**; nếu để mặc định thì sẽ chỉ có một node dành cho control plane, còn nếu bật thì có thể thêm nhiều node hơn cho control plane
  ![04](../K8s/images/04.png)
* Chọn **Add now** để cấu hình thêm SSH Key -> Thêm public SSH key và chọn Key tương ứng
  ![05](../K8s/images/05.png)
  ![06](../K8s/images/06.png)
* Điền tên tại **Cluster Name** và bấm **Review & Create Cluster**
  ![07](../K8s/images/07.png)

## 2. Truy cập và quản lí K8s Cluster
* Sau khi khởi tạo thành công K8s Cluster thì có thể tải config file và truy cập - quản lí thông qua CLI hoặc K8s Dashboard UI
  ![08](../K8s/images/08.png)
  ![09](../K8s/images/09.png)
  ![010](../K8s/images/010.png)
* Để nâng cấp phiên bản K8s, cần thực hiện theo các bước dưới đây:
  ![037](../K8s/images/037.png)
* Sau đó trạng thái của cluster sẽ chuyển qua **Upgrading** 
  ![038](../K8s/images/038.png)
  ![039](../K8s/images/039.png)
* Hoàn thành nâng cấp
  ![040](../K8s/images/040.png)
  
  
## 3. Auto scaling
### Kích hoạt
* Chọn biểu tượng **Scale Kubernetes Cluster**
![011](../K8s/images/011.png)
* Điền giới hạn số lượng **Worker Node** (tối thiểu, tối đa) cho cụm và bấm **Submit**
![012](../K8s/images/012.png)
![013](../K8s/images/013.png)

### Kiểm thử 
* Khởi tạo ứng dụng web **php-apache** để tiếp nhận các luồng truy cập
```
kubectl apply -f https://k8s.io/examples/application/php-apache.yaml
```
![014](../K8s/images/014.png)
![015](../K8s/images/015.png)
* Thiết lập định mức tài nguyên CPU tối thiểu cho mỗi pod **php-apache** là 200m
```
kubectl set resources deployment php-apache --requests=cpu=200m
```
* Thiết lập điều kiện HPA cho pod **php-apache**
```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```
![016](../K8s/images/016.png)
* Khởi tạo các pod **load-generator** để tạo lưu lượng đến **php-apache**
```
kubectl apply -f load-generator.yaml
```
![017](../K8s/images/017.png)
![018](../K8s/images/018.png)
![019](../K8s/images/019.png)
* Khi đó, tính năng **Auto Scaling** đã hoạt động để đáp ứng mức tài nguyên yêu cầu tăng cao cho các pods **php-apache**, **load-generator**
![020](../K8s/images/020.png) 
![021](../K8s/images/021.png)
![022](../K8s/images/022.png)
* Khi giảm lưu lượng HTTP thông qua giảm số lượng pod **load-generator** -> giảm mức yêu cầu tài nguyên từ các worker node -> **Auto Scaling** sẽ tự động xóa các worker node không cần thiết khỏi cluster
![023](../K8s/images/023.png)
![024](../K8s/images/024.png)

> Lưu ý: tính năng Auto Scaling chưa thể hoạt động được trên phiên bản Kubernetes v1.33.1, v1.32.5, v1.33.1
## 4. Load balancer
* Cài đặt Nginx Ingress Controller
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml
```
![025](../K8s/images/025.png)
![026](../K8s/images/026.png)
* Chờ cấp phát public IP
![027](../K8s/images/027.png)
> Ingress Controller đã tự động giao tiếp với VNData Cloud để tạo Load Balancer và nhận Public IP
* Triển khai 2 Ứng dụng Web Server (Nginx & Apache):
  * Tạo app Nginx:
    ```
    kubectl create deployment web-nginx --image=nginx
    kubectl expose deployment web-nginx --port=80 --type=ClusterIP
    ```
  * Tạo App Apache:
    ```
    kubectl create deployment web-apache --image=httpd
    kubectl expose deployment web-apache --port=80 --type=ClusterIP
    ```
  ![028](../K8s/images/028.png)
  ![029](../K8s/images/029.png)
  ![030](../K8s/images/030.png)
* Tạo file cấu hình ``ingress.yaml``  
  ![031](../K8s/images/031.png)
* Áp dụng cấu hình trên vào cluster K8s
  ![032](../K8s/images/032.png)
  ![033](../K8s/images/033.png)
* Cấu hình tên miền cục bộ  
  ![034](../K8s/images/034.png)
* Kiểm thử thông qua việc truy cập 2 tên miền cục bộ trên
  ![035](../K8s/images/035.png)
  ![036](../K8s/images/036.png)
  

