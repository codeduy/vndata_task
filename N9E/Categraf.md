# Các bước cài đặt và cấu hình agent Categraf khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Tải tệp categraf-v0.4.22-linux-amd64.tar.gz từ [Categraf release](https://github.com/flashcatcloud/categraf/releases).
```
wget https://github.com/flashcatcloud/categraf/releases/download/v0.4.22/categraf-slim-v0.4.22-linux-amd64.tar.gz
```
* Bước 2: Giải nén tệp categraf-v0.4.22-linux-amd64.tar.gz
```
tar -xf categraf-v0.4.22-linux-amd64.tar.gz
```
* Bước 3: Di chuyển tệp đến thư mục /opt
```
mv categraf-v0.4.22-linux-amd64 /opt
```
* Bước 4: Truy cập thư mục /opt
```
cd /opt
```
* Bước 5: Đổi tên để dễ quản lí.
```
mv categraf-v0.4.22-linux-amd64 categraf
```
* Bước 6: Cấp quyền thực thi cho file categraf
```
sudo chmod +x /opt/categraf/categraf
```
* Bước 7: Truy cập thư mục cấu hình của categraf
```
cd categraf/conf
```
* Bước 8: Mở file cấu hình.
```
nano config.toml
```
* Bước 9: Chỉnh sửa cấu hình.
```
[global]
# whether print configs
print_configs = false

# add label(agent_hostname) to series
# "" -> auto detect hostname
# "xx" -> use specified string xx
# "$hostname" -> auto detect hostname
# "$ip" -> auto detect ip
# "$sn" -> auto detect bios serial number
# "$hostname-$ip" -> auto detect hostname and ip to replace the vars
hostname = ""

# will not add label(agent_hostname) if true
omit_hostname = false

# global collect interval, unit: second
interval = 15

# input provider settings; optional: local / http
providers = ["local"]

# The concurrency setting controls the number of concurrent tasks spawned for each input. 
# By default, it is set to runtime.NumCPU() * 10. This setting is particularly useful when dealing
# with configurations that involve extensive instances of input like ping, net_response, or http_response.
# As multiple goroutines run simultaneously, the "ResponseTime" metric might appear larger than expected. 
# However, utilizing the concurrency setting can help mitigate this issue and optimize the response time.
concurrency = -1

# Setting http.ignore_global_labels = true if disabled report custom labels
[global.labels]
# region = "shanghai"
# env = "localhost"
# sn = "$sn"

[log]
# file_name is the file to write logs to
file_name = "stdout"

# options below will not be work when file_name is stdout or stderr
# max_size is the maximum size in megabytes of the log file before it gets rotated. It defaults to 100 megabytes.
max_size = 100
# max_age is the maximum number of days to retain old log files based on the timestamp encoded in their filename.  
max_age = 1
# max_backups is the maximum number of old log files to retain.  
max_backups = 1
# local_time determines if the time used for formatting the timestamps in backup files is the computer's local time.  
local_time = true
# Compress determines if the rotated log files should be compressed using gzip. 
compress = false

#[writer_opt]
#batch = 1000
#chan_size = 1000000

#[[writers]]
#url = "http://127.0.0.1:19000/prometheus/v1/write"

## Optional TLS Config
# tls_min_version = "1.2"
# tls_ca = "/etc/categraf/ca.pem"
# tls_cert = "/etc/categraf/cert.pem"
# tls_key = "/etc/categraf/key.pem"
## Use TLS but skip chain & host verification
# insecure_skip_verify = true

# Basic auth username
#basic_auth_user = ""

# Basic auth password
#basic_auth_pass = ""

## Optional headers
# headers = ["X-From", "categraf", "X-Xyz", "abc"]

# timeout settings, unit: ms
#timeout = 5000
#dial_timeout = 2500
#max_idle_conns_per_host = 100

[http]
enable = false
address = ":9100"
print_access = false
run_mode = "release"
ignore_hostname = false
agent_host_tag = ""
ignore_global_labels = false

[ibex]
enable = false
## ibex flush interval
interval = "1000ms"
## n9e ibex server rpc address
servers = ["127.0.0.1:20090"]
## temp script dir
meta_dir = "./meta"

[heartbeat]
enable = true

# report os version cpu.util mem.util metadata
url = "http://127.0.0.1:19000/v1/n9e/heartbeat"

# interval, unit: s
interval = 10

# Basic auth username
basic_auth_user = ""

# Basic auth password
basic_auth_pass = ""

## Optional headers
# headers = ["X-From", "categraf", "X-Xyz", "abc"]

# timeout settings, unit: ms
timeout = 5000
dial_timeout = 2500
max_idle_conns_per_host = 100

[prometheus]
enable = false
scrape_config_file = "/path/to/in_cluster_scrape.yaml"
## log level, debug warn info error
log_level = "info"
## wal file storage path ,default ./data-agent
# wal_storage_path = "/path/to/storage"
## wal reserve time duration, default value is 2 hour
# wal_min_duration = 2
```
* Bước 10: Tạo file categraf.service
```
nano /etc/systemd/system/categraf.service
```
với nội dung sau:
```
[Unit]
Description=Categraf Metrics Agent
After=network.target

[Service]
ExecStart=/opt/categraf/categraf --configs /opt/categraf/conf
WorkingDirectory=/opt/categraf
Restart=always
RestartSec=5
SyslogIdentifier=categraf

[Install]
WantedBy=multi-user.target
```
* Bước 11: Chạy các lệnh sau để enable và start dịch vụ categraf.service
```
sudo systemctl daemon-reload
systemctl enable --now categraf
```
* Bước 12: Kiểm tra trạng thái dịch vụ categraf.service
```
systemctl status categraf
```
```
journalctl -u categraf -f
```




