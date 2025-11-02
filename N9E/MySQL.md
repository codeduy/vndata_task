# Các bước cài đặt và cấu hình MySQL khi triển khai [N9E Architecture](../N9E/General/general.md)

* Bước 1: Cài đặt MySQL Server
```
sudo apt update
sudo apt install -y mysql-server
```
* Bước 2: Khởi động & bật tự khởi động
```
sudo systemctl enable --now mysql
sudo systemctl status mysql
```
* Bước 3: Truy cập vào mysql dưới quyền root
```
sudo mysql
```
* Bước 4: Tạo database riêng cho N9E với tên là n9e_v6 với username:password tương ứng là n9e:StrongN9e#2025 với đầy đủ quyền truy cập vào n9e_v6
```
CREATE DATABASE n9e_v6 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'n9e'@'127.0.0.1' IDENTIFIED BY 'StrongN9e#2025';
GRANT ALL PRIVILEGES ON n9e_v6.* TO 'n9e'@'127.0.0.1';
FLUSH PRIVILEGES;
EXIT;
```

