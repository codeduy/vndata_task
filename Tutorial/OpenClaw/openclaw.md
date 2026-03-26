# Hướng dẫn Cài đặt & Cấu hình AI Agent OpenClaw

## 1. Chuẩn bị
* Một VPS (Fresh OS) chạy Ubuntu 22.04 với cấu hình là 2 cores - 4 GB RAM
* [Google AI Studio](https://aistudio.google.com/u/2/api-keys) API Key
* Telegram Bot Token (Tạo bot và lấy API Token thông qua [@BotFather](https://telegram.me/BotFather#))

## 2. Cài đặt OpenClaw
* Chạy lệnh sau để tiến hành cài đặt OpenClaw:
  ```
  curl -fsSL https://openclaw.ai/install.sh | bash
  ```
  ![](../OpenClaw/images/1.png)
* Chọn **Yes** -> **QuickStart**
  ![](../OpenClaw/images/2.png)
  ![](../OpenClaw/images/3.png)  
* Chọn **Google** -> **Google Gemini API key** -> Nhập API Key
  ![](../OpenClaw/images/4.png)
  ![](../OpenClaw/images/5.png)
  ![](../OpenClaw/images/6.png)
* Di chuột lên xuống và nhấp **Enter** để chọn model măc định
  ![](../OpenClaw/images/7.png)
* Nhấp **Enter** để chọn **Telegram (Bot API)** -> **Enter Telegram bot token**
  ![](../OpenClaw/images/8.png)
  ![](../OpenClaw/images/9.png)
  ![](../OpenClaw/images/10.png)
* Chọn **DuckDuckGo Search** tại **Search provider**
  ![](../OpenClaw/images/11.png)
* Tại **Configure skills now?** -> Chọn **No**
  ![](../OpenClaw/images/12.png)
* Tại **Enable hooks?** -> Nhấp **Space** để chọn tùy chọn này và nhấp **Enter**
  ![](../OpenClaw/images/13.png)
* Chọn **Hatch in TUI (recommended)**
  ![](../OpenClaw/images/14.png)
* Tại giao diện này, có thể chat thử vài câu với bot. Nếu có phản hồi là OK
  ![](../OpenClaw/images/15.png)
  Dùng tổ hợp phím **Ctrl + C** để thoát khỏi giao diện **TUI** này
* Tiếp theo chạy các lệnh sau để giới hạn tài khoản Telegram truy cập vào bot
  ```
  openclaw config set channels.telegram.allowFrom '["ĐIỀN_ID_LẤY_TỪ_@userinfobot"]'
  ```
  ```
  openclaw config set channels.telegram.dmPolicy "allowlist"
  ```
  ```
  openclaw gateway restart
  ```
  ![](../OpenClaw/images/16.png)
* Khi này thì đã có thể tương tác với bot qua Telegram
  ![](../OpenClaw/images/17.png)
* Nếu muốn sửa lại cấu hình OpenClaw thì có thể chạy lệnh sau
  ```
  openclaw configure
  ```
  ![](../OpenClaw/images/18.png)
* Hoặc tiện hơn thì có thể truy cập OpenClaw Dashboard trên thiết bị cá nhân (PC)
  * Chạy lệnh sau trên PC để login và tạo tunnel vào VPS chạy OpenClaw
    ```
    ssh -L 18789:127.0.0.1:18789 root@IP_CỦA_SERVER_UBUNTU
    ```
  * Trên VPS chạy lệnh sau để lấy link login Dashboard kèm token xác thực
    ```
    openclaw dashboard --no-open
    ```
    ![](../OpenClaw/images/19.png)
  * Truy cập link trên để vào trang OpenClaw Control Dashboard
    ![](../OpenClaw/images/20.png)
