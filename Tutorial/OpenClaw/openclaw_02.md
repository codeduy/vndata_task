# Hướng dẫn Cài đặt & Cấu hình AI Agent OpenClaw

## 1. Chuẩn bị
* Một VPS (Fresh OS) chạy Ubuntu 22.04 với cấu hình là 2 cores - 4 GB RAM
* [Google AI Studio](https://aistudio.google.com/u/2/api-keys) API Key
* Telegram Bot Token (Tạo bot và lấy API Token thông qua [@BotFather](https://telegram.me/BotFather#))

## 2. Cài đặt và Cấu hình OpenClaw
* Chạy lệnh sau để tiến hành cài đặt OpenClaw:
  ```
  curl -fsSL https://openclaw.ai/install.sh | bash
  ```
  ![01](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/01.png)
* Chọn **Yes** -> **QuickStart**
  ![02](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/02.png)
  ![03](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/03.png)  
* Chọn **Google** -> **Google Gemini API key** -> Nhập API Key
  ![04](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/04.png)
  ![05](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/05.png)
  ![06](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/06.png)
* Điều hướng mũi tên lên xuống và nhấp **Enter** để chọn model mặc định
  ![07](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/07.png)
* Nhấp **Enter** để chọn **Telegram (Bot API)** -> **Enter Telegram bot token**
  ![08](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/08.png)
  ![09](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/09.png)
  ![10](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/10.png)
* Chọn **DuckDuckGo Search** tại **Search provider**
  ![11](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/11.png)
* Tại **Configure skills now?** -> Chọn **No**
  ![12](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/12.png)
* Tại **Enable hooks?** -> Nhấp **Space** để chọn tùy chọn **Skip for now** và nhấp **Enter**
  ![13](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/13.png)
* Chọn **Hatch in TUI (recommended)**
  ![14](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/14.png)
* Tại giao diện này, có thể chat thử vài câu với bot. Nếu có phản hồi là OK
  ![15](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/15.png)
  Dùng tổ hợp phím **Ctrl + C** để thoát khỏi giao diện **TUI** này
* Tiếp theo chạy các lệnh sau để giới hạn tài khoản Telegram có thể chat với bot
  ```
  openclaw config set channels.telegram.allowFrom '["ĐIỀN_ID_LẤY_TỪ_@userinfobot"]'
  ```
  ```
  openclaw config set channels.telegram.dmPolicy "allowlist"
  ```
  ```
  openclaw gateway restart
  ```
  ![16](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/16.png)
* Khi này thì đã có thể tương tác với bot qua Telegram
  ![17](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/17.png)
* Nếu muốn sửa lại cấu hình OpenClaw thì có thể chạy lệnh sau
  ```
  openclaw configure
  ```
  ![18](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/18.png)
* Hoặc tiện hơn thì có thể truy cập OpenClaw Dashboard trên thiết bị cá nhân (PC)
  * Chạy lệnh sau trên PC để login và tạo tunnel vào VPS chạy OpenClaw
    ```
    ssh -L 18789:127.0.0.1:18789 root@IP_CỦA_SERVER_UBUNTU
    ```
  * Trên VPS chạy lệnh sau để lấy link login Dashboard kèm token xác thực
    ```
    openclaw dashboard --no-open
    ```
    ![19](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/19.png)
  * Truy cập link trên để vào trang OpenClaw Control Dashboard
    ![20](https://s3-hcm-r2.s3cloud.vn/img-blog/openclaw/20.png)
