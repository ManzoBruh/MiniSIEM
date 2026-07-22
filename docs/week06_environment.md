# Thiết lập môi trường — Tuần 6

## 1. Yêu cầu hệ thống

| Thành phần | Yêu cầu tối thiểu | Khuyến nghị |
|---|---|---|
| OS (Server) | Windows 10/11, Linux, macOS | Windows 11 hoặc Ubuntu 22.04 |
| RAM (Server) | 8GB | 16GB+ |
| CPU | 4 cores | 8 cores+ |
| Storage | 50GB free | 100GB+ |
| VirtualBox | 7.x | Mới nhất |
| Ubuntu-live-server | 24.04.4 | Được tối ưu cho tương thích
| OS (Agent) | Windows 10/11, Ubuntu 18.04+ | Windows 11 / Ubuntu 22.04 |

## 2. Cài đặt Ubuntu Server

Tải về Virtual Box  
https://download.virtualbox.org/virtualbox/7.2.12/VirtualBox-7.2.12-174389-Win.exe

Tải về Ubuntu Server LTS
https://ubuntu.com/download/server/thankyou?version=24.04.4&architecture=amd64&lts=true

Chọn ISO image là phần Ubuntu mình vừa tải về

- Để Username, hệ thống theo ý muốn
- Chọn vào phần Specify Virtual Hardware 
- Để base memory 9GB và CPU là 4
- Chuyển sang phần Specify Virtual Hard Disk thì sẽ đặt là 50 GB tối thiểu
*Lưu ý: Khi vào trong hệ thống chọn cái Icon Mạng bên góc bên phải và chọn Bridge Adapter 


# 2.1 Cài Đặt Wazuh

Sau khi hệ thống hoàn thành set up, giờ chúng ta curl về Wazuh cho hệ thống Server

curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh –a

Chờ một thời gian để hệ thống cài đặt xong Wazuh.manager, dashboard và indexer

# 3. Kiểm tra
Hệ thống sẽ hiện lên thông tin giúp chúng ta xác thực bao gồm:
    - Đường dẫn tới Wazuh Dashboard https://<wazuh-dashboard-ip>:443
    - Tài khoản và mật khẩu

*Lưu ý: Nên lưu lại mật khẩu nhưng trong trường hợp bị quên hoặc muốn cài đặt lại đọc qua hướng dẫn của Wazuh để tham khảo thêm


## 4. Cấu trúc thư mục dự án

````

MiniSIEM/
│
├── README.md                          
├── .gitignore
├── .env.example
├── docs/
├── evidence/
├── wazuh/
│   └── config/
├── suricata/
````
## 5. Git Workflow

# Clone project repo
git clone https://github.com/ManzoBruh/MiniSIEM.git
cd MiniSIEM
