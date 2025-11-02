# Các bước cài đặt và cấu hình Grafana Alloy - Observability Agent khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Thêm repository chính thức của Grafana
```
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```
* Bước 2: Cập nhật danh sách package
```
sudo apt-get update
```
* Bước 3: Cài đặt Grafana Alloy
```
sudo apt-get install alloy
```
* Bước 4: Mở file cấu hình chính
```
nano /etc/alloy/config.alloy
```
* Bước 5: Cấu hình Alloy để gửi Metrics & Logs
```
// =======================
// METRICS -> Prometheus
// =======================

prometheus.exporter.unix "local_system" { }

prometheus.scrape "scrape_metrics" {
  targets         = prometheus.exporter.unix.local_system.targets
  forward_to      = [prometheus.relabel.filter_metrics.receiver]
  scrape_interval = "10s"
}

prometheus.relabel "filter_metrics" {
  rule {
    action        = "drop"
    source_labels = ["env"]
    regex         = "dev"
  }

  forward_to = [prometheus.remote_write.metrics_service.receiver]
}

prometheus.remote_write "metrics_service" {
    endpoint {
        url = "http://127.0.0.1:19000/prometheus/v1/write"

        // basic_auth {
        //   username = "admin"
        //   password = "admin"
        // }
    }
}

// =======================
// LOGS -> Loki
// =======================

loki.source.file "kernel_scrape" {
  targets = [
    {
      "__path__" = "/var/log/kern.log",
      "job"      = "kernel",
      "host"     = "n9e-edge",
    },
  ]

  forward_to     = [loki.write.grafana_loki.receiver]
  tail_from_end  = false
}

loki.write "grafana_loki" {
  endpoint {
    url = "http://127.0.0.1:3100/loki/api/v1/push"
  }
}
```
* Bước 6: Kích hoạt và khởi chạy Alloy
```
sudo systemctl enable --now alloy.service
```
* Bước 7: Kiểm tra trạng thái và theo dõi log
```
sudo systemctl status alloy
sudo journalctl -u alloy -f
```





