# Các bước cài đặt và cấu hình MySQL khi triển khai [N9E Architecture](../N9E/General/general.md)

```
sudo apt update
sudo apt install -y mysql-server
```

```
sudo systemctl enable --now mysql
sudo systemctl status mysql
```

```
sudo mysql
```

```
CREATE DATABASE n9e_v6 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'n9e'@'127.0.0.1' IDENTIFIED BY 'StrongN9e#2025';
GRANT ALL PRIVILEGES ON n9e_v6.* TO 'n9e'@'127.0.0.1';
FLUSH PRIVILEGES;
EXIT;
```
