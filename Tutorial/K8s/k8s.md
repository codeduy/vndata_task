# Hướng dẫn triển khai K8s Cluster
## 1. Khởi tạo K8s Cluster
* Tại phần **Virtual Machines** -> chọn **Kubernetes** -> chọn **Create New**
  ![01](../K8s/images/01.png)
* Chọn phiên bản **Kubernetes** tại **Select Version**, **Network** tại **Select Network**, cấu hình của mỗi node trong cluster tại **Choose Cluster Capacity** 
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

## 2. Access K8s

## 3. Auto scaling

## 4. Load balancer
