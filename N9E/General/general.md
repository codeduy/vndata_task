# SƠ LƯỢC VỀ N9E

* **N9E** là dự án mã nguồn mở có khả năng tích hợp các data sources tương tự như Grafana Dashboard nhưng nổi trội hơn về phần cảnh báo (alerting)
* **N9E** có thể truy vấn dữ liệu từ các data sources mà tích hợp vào nó như Promethues, Grafana Loki, ElasticSearch,... phục vụ cho việc tạo sự kiện cảnh báo(alarm event) và gửi thông báo về các kênh thông báo phổ biến như Slack, Telegram, Discord,...

## Mô hình đơn giản đã triển khai 

![](../images/N9E_Architecture.png)

## Chi tiết các thành phần trong mô hình

* Luồng **A**: **n9e-edge** sẽ đồng bộ các thông tin mà **n9e-edge** lưu trữ ở **MySQL** như notify_rule, datasource, alert_rule,... phục vụ cho việc cảnh báo dựa trên các rules khi bị mất kết nối với **n9ecenter** (nhưng sẽ không thể buffer alert event để gửi lại cho **MySQL** tại **n9ecenter** khi kết nối đã được khôi phục).
* Luồng **B**: **n9ecenter** sẽ dựa trên thông tin datasource đã cấu hình với **Prometheus** mà sẽ định kì truy cấp các dữ liệu metrics mà **Prometheus** lưu trữ dựa trên các cú pháp PromQL (Prometheus Query Language) đã được cấu hình sẵn phục vụ cho custom query, alert và dashboard.
* Luồng **C**: **n9ecenter** sẽ dựa trên thông tin datasource đã cấu hình với **Loki** mà sẽ định kì truy cấp các dữ liệu logs mà **Loki** lưu trữ dựa trên các cú pháp LogQL đã được cấu hình sẵn phục vụ cho custom query, alert.
* Luồng **D**: 
### Tham khảo thêm
* [Details N9E architecture](https://lucid.app/lucidchart/f328617b-e8ff-49a5-85c0-2bc6577aec5b/edit?invitationId=inv_36d7034b-1599-4e6f-bb9e-c45f04a3ede2)
