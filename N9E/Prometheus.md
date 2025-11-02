# Các bước cài đặt và cấu hình Prometheus - Monitoring System khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Tải gói Prometheus cho Linux tại [Prometheus Release](https://github.com/prometheus/prometheus/releases/)
```
wget https://github.com/prometheus/prometheus/releases/download/v3.7.3/prometheus-3.7.3.linux-amd64.tar.gz
```
* Bước 2: Giải nén file Prometheus
```
tar -xf prometheus-3.7.3.linux-amd64.tar.gz
```
* Bước 3: Truy cập vào thư mục giải nén
```
cd prometheus-3.7.3.linux-amd64
```
* Bước 4: Di chuyển các file cần thiết đến thư mục chuẩn /opt
```
mkdir -p /opt/prometheus && mv prometheus prometheus.yml promtool /opt/prometheus && cd && cd /opt/prometheus
```
* Bước 5: Mở file cấu hình Prometheus
```
nano prometheus.yml 
```
* Bước 6: Cấu hình Prometheus
```
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
       # The label name is added as a label `label_name=<label_value>` to any timeseries scraped from this config.
        labels:
          app: "prometheus"
```
* Bước 7: Tạo file service cho Prometheus
```
nano /etc/systemd/system/prometheus.service
```
* Bước 8: Dán nội dung cấu hình systemd
```
[Unit]
Description=Prometheus Monitoring Service
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/opt/prometheus
ExecStart=/opt/prometheus/prometheus \
  --config.file=/opt/prometheus/prometheus.yml \
  --storage.tsdb.path=/opt/prometheus/data \
  --web.listen-address=:9090 \
  --web.enable-lifecycle \
  --web.enable-remote-write-receiver
Restart=always
RestartSec=5
LimitNOFILE=65535
SyslogIdentifier=prometheus

[Install]
WantedBy=multi-user.target
```
* Bước 9: Kích hoạt và khởi chạy Prometheus
```
systemctl enable --now prometheus
```
* Bước 10: Kiểm tra trạng thái hoạt động
```
systemctl status prometheus
journalctl -u prometheus -f
```


