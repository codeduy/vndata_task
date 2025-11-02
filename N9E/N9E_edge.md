# Các bước cài đặt và cấu hình N9E trên n9e-edge VM khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Tạo thư mục chứa và truy cập
```
cd /opt && mkdir n9e-edge && cd n9e-edge
```
* Bước 2: Tải file binary của N9E tại [N9E Release](https://github.com/ccfos/nightingale/releases)
```
wget https://github.com/ccfos/nightingale/releases/download/v8.4.0/n9e-v8.4.0-linux-amd64.tar.gz
```
* Bước 3: Giải nén gói vừa tải
```
tar -xf n9e-v8.4.0-linux-amd64.tar.gz
```
* Bước 4: Xóa file nén để dọn dẹp
```
rm -rf n9e-v8.4.0-linux-amd64.tar.gz
```
* Bước 5: Mở file cấu hình chính
```
cd etc/edge && nano edge.toml
```
* Bước 6: Cấu hình N9E Edge
```
[Global]
RunMode = "release"

[CenterApi]
Addrs = ["http://10.20.230.2:17000"]
BasicAuthUser = "user001"
BasicAuthPass = "ccc26da7b9aba533cbb263a36c07dcc5"
# unit: ms
Timeout = 9000

[Log]
# log write dir
Dir = "logs"
# log level: DEBUG INFO WARNING ERROR
Level = "INFO"
# stdout, stderr, file
Output = "stdout"
# # rotate by time
# KeepHours = 4
# # rotate by size
# RotateNum = 3
# # unit: MB
# RotateSize = 256

[HTTP]
# http listening address
Host = "0.0.0.0"
# http listening port
Port = 19000
# https cert file path
CertFile = ""
# https key file path
KeyFile = ""
# whether print access log
PrintAccessLog = false
# whether enable pprof
PProf = false
# expose prometheus /metrics?
ExposeMetrics = true
# http graceful shutdown timeout, unit: s
ShutdownTimeout = 30
# max content length: 64M
MaxContentLength = 67108864
# http server read timeout, unit: s
ReadTimeout = 20
# http server write timeout, unit: s
WriteTimeout = 40
# http server idle timeout, unit: s
IdleTimeout = 120

[HTTP.APIForAgent]
Enable = true 
# [HTTP.APIForAgent.BasicAuth]
# user001 = "ccc26da7b9aba533cbb263a36c07dcc5"

[HTTP.APIForService]
Enable = false
[HTTP.APIForService.BasicAuth]
user001 = "ccc26da7b9aba533cbb263a36c07dcc5"

[Alert]
[Alert.Heartbeat]
# auto detect if blank
IP = ""
# unit ms
Interval = 1000
EngineName = "edge"

# [Alert.Alerting]
# NotifyConcurrency = 10

[Pushgw]
# use target labels in database instead of in series
LabelRewrite = true
# # default busigroup key name
# BusiGroupLabelKey = "busigroup"
ForceUseServerTS = true

# [Pushgw.DebugSample]
# ident = "xx"
# __name__ = "xx"

# [Pushgw.WriterOpt]
# QueueMaxSize = 1000000
# QueuePopSize = 1000

[[Pushgw.Writers]] 
# Url = "http://127.0.0.1:8480/insert/0/prometheus/api/v1/write"
Url = "http://127.0.0.1:9090/api/v1/write"
# Basic auth username
BasicAuthUser = ""
# Basic auth password
BasicAuthPass = ""
# timeout settings, unit: ms
Headers = ["X-From", "n9e-edge"]
Timeout = 10000
DialTimeout = 3000
TLSHandshakeTimeout = 30000
ExpectContinueTimeout = 1000
IdleConnTimeout = 90000
# time duration, unit: ms
KeepAlive = 30000
MaxConnsPerHost = 0
MaxIdleConns = 100
MaxIdleConnsPerHost = 100
## Optional TLS Config
# UseTLS = false
# TLSCA = "/etc/n9e/ca.pem"
# TLSCert = "/etc/n9e/cert.pem"
# TLSKey = "/etc/n9e/key.pem"
# InsecureSkipVerify = false
# [[Writers.WriteRelabels]]
# Action = "replace"
# SourceLabels = ["__address__"]
# Regex = "([^:]+)(?::\\d+)?"
# Replacement = "$1:80"
# TargetLabel = "__address__"

[Ibex]
Enable = false
RPCListen = "0.0.0.0:20090"

# n9e-edge cannot directly reuse the redis that n9e relies on at the center.
# It needs to deploy a separate redis in the edge region for n9e-edge to use.
[Redis]
# address, ip:port or ip1:port,ip2:port for cluster and sentinel(SentinelAddrs)
Address = "127.0.0.1:6379"
# Username = ""
Password = "StrongEdgeRedis#2025"
DB = 0
# UseTLS = false
# TLSMinVersion = "1.2"
# standalone cluster sentinel
RedisType = "standalone"
# Mastername for sentinel type
# MasterName = "mymaster"
# SentinelUsername = ""
# SentinelPassword = ""
```
* Bước 7: Tạo service file cho N9E Edge
```
nano /etc/systemd/system/n9e-edge.service
```
* Bước 8: Dán nội dung cấu hình systemd
```
Unit]
Description=Nightingale Edge Service
After=network-online.target redis-server.service
Wants=network-online.target

[Service]
Type=simple
WorkingDirectory=/opt/n9e-edge/
ExecStart=/opt/n9e-edge/n9e-edge -configs /opt/n9e-edge/etc/edge
Restart=on-failure
RestartSec=5
LimitNOFILE=65535

StandardOutput=journal
StandardError=journal
SyslogIdentifier=n9e-edge

[Install]
WantedBy=multi-user.target
```
* Bước 9: Enable và khởi chạy N9E Edge
```
systemctl enable --now n9e-edge
```
* Bước 10: Kiểm tra hoạt động
```
systemctl status n9e-edge
journalctl -u n9e-edge -f
```

