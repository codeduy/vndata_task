# Các bước cài đặt và cấu hình Redis khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Cập nhật gói và cài đặt Redis Server
```
sudo apt update
sudo apt install -y redis-server
```
* Bước 2: Kích hoạt và khởi chạy Redis
```
sudo systemctl enable --now redis-server
sudo systemctl status redis-server
```
* Bước 3: Mở file cấu hình Redis
```
sudo nano /etc/redis/redis.conf
```
* Bước 4: Tìm dòng cấu hình mật khẩu mặc định
```
# requirepass foobared
```
* Bước 5: Thêm hoặc chỉnh sửa dòng mật khẩu
```
requirepass StrongRedis#2025
```
* Bước 6: Khởi động lại dịch vụ Redis
```
sudo systemctl restart redis-server
```
* Bước 7: Kiểm tra kết nối Redis và xác thực
```
redis-cli -a 'StrongRedis#2025' ping
```
* Bước 8: Xác nhận kết quả
```
PONG
```
> Nếu redis trả về PONG thì redis đã được cấu hình đúng và hoạt động tốt
