# Các bước cài đặt và cấu hình Redis khi triển khai [N9E Architecture](../N9E/General/general.md)

```
sudo apt update
sudo apt install -y redis-server
```

```
sudo systemctl enable --now redis-server
sudo systemctl status redis-server
```

```
sudo nano /etc/redis/redis.conf
```

```
# requirepass foobared
```

```
requirepass StrongRedis#2025
```

```
sudo systemctl restart redis-server
```

```
redis-cli -a 'StrongRedis#2025' ping
```
```
PONG
```
