# Template trong Proxmox

## Sơ lược về template
* Template là một VM đã cài OS + App hoàn chỉnh (aaPanel, Qemu Guest Agent, monitoring agent, OS config tùy chỉnh,...) --> đóng băng lại thành một template dùng để clone - triển khai thành nhiều VM; giúp tiết kiệm thời gian, giảm lỗi so với việc cấu hình thủ công từng VM mà không dùng template.
* Có hai loại Clone là Linked Clone và Full Clone:
  * Linked Clone: clone thành VM dùng chung imageOS + App ở định dạng .qcow2/raw nằm trong **Hard Disk** của template. Còn **Hard Disk** của VM mới clone ra thì chỉ dùng để lưu những thay đổi của OS/App trong VM đó so với template.
    > Ở tùy chọn này thì VM sẽ không thể thực hiện live migrate disk được; nếu muốn thực hiện thì cần migrate cả phần template theo qua ổ đĩa lưu trữ tương ứng của VM.
  * Full Clone: sao chép toàn bộ mọi thứ từ Template qua VM mới, VM và template sẽ độc lập với nhau - không còn liên kết ở bất cứ dữ liệu nào nữa.
