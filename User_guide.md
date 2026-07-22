# Hướng Dẫn Vận Hành Hệ Thống MiniSIEM (End-to-End User Guide)

Tài liệu hướng dẫn vận hành toàn diện này được thiết kế để triển khai và kiểm thử hệ thống MiniSIEM. Tài liệu bao quát từ khâu kiến trúc ảo hóa, lựa chọn hệ điều hành, cho đến cấu hình luồng dữ liệu (Wazuh + Suricata) và các kịch bản demo.

---

## Phần 1: Xây dựng Môi trường Ảo hóa & Lựa chọn OS

Để mô phỏng một hệ thống mạng thực tế (MiniSIEM), hệ thống được chia thành hai phần: Máy chủ trung tâm (phân tích log) và Máy trạm (mục tiêu bảo vệ).

### 1. Lựa chọn Hệ điều hành

* **Wazuh Manager (Máy chủ giám sát):** Lựa chọn **Ubuntu Server 24.04**. 
  * *Lý do:* Đây là hệ điều hành mã nguồn mở, tối ưu tài nguyên (không cần giao diện đồ họa GUI), độ ổn định cao và là môi trường được hỗ trợ tốt nhất cho các công cụ bảo mật như Wazuh.
* **Máy trạm (Nạn nhân & Kẻ tấn công):** Lựa chọn **Windows 10/11**.
  * *Lý do:* Phản ánh đúng môi trường máy tính phổ biến của người dùng cuối trong thực tế, dễ dàng cài đặt Suricata Windows và Wazuh Agent.

### 2. Cấu hình VirtualBox Mạng (Quan trọng)

Để Ubuntu Server và Windows có thể "nhìn thấy" nhau và giao tiếp như hai máy vật lý trên cùng một mạng, bạn phải thiết lập mạng ảo hóa đúng cách:

1. Trong VirtualBox, chọn máy ảo Ubuntu Server -> **Settings** -> **Network**.
2. Đổi **Attached to** thành **Bridged Adapter**.
3. Tại mục **Name**, chọn đúng card mạng (Wi-Fi hoặc Ethernet) mà máy tính vật lý của bạn đang dùng để kết nối internet.
4. *(Xử lý lỗi mất mạng)*: Nếu Ubuntu bị mất kết nối IP, hãy gõ lệnh sau để làm mới cấu hình mạng:
   ```bash
   sudo ip addr flush dev enp0s3
   sudo netplan apply


## Phần 2: Cài Đặt Trung Tâm Giám Sát (Wazuh Manager)

Thực hiện các lệnh sau trên **Ubuntu Server 24.04** để cài đặt trọn bộ Wazuh (bao gồm Wazuh Indexer, Server, và Dashboard) thông qua kịch bản cài đặt tự động (Installation Assistant):

### 2.1 Cài đặt tự động
```bash
curl -sO [https://packages.wazuh.com/4.x/wazuh-install.sh](https://packages.wazuh.com/4.x/wazuh-install.sh)
sudo bash wazuh-install.sh -a
```


(Hệ thống sẽ tự động cấu hình trong khoảng 15-20 phút. Khi hoàn tất, màn hình sẽ hiển thị thông tin đăng nhập bao gồm URL, Username admin và Password)

### 2.2 Đăng nhập Dashboard

Mở trình duyệt web trên máy tính thật, truy cập địa chỉ IP của Ubuntu Server (VD: https://<IP_CỦA_UBUNTU>).

Đăng nhập bằng tài khoản được cung cấp từ bước trên.


## Phần 3: Cài Đặt Máy Trạm & Cảm Biến (Suricata + Wazuh Agent)


Thực hiện các bước sau trên máy Windows (Nạn nhân). Giả định bạn lưu bộ cài Suricata tại C:\Users\Admin\MiniSIEM\suricata.

### 3.1 Thiết lập Môi trường Suricata
Mở PowerShell (Run as Administrator) và chạy lệnh tạo thư mục log cho Suricata:

```bash
New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"

```

### 3.2 Tải Bộ luật Nhận dạng (Emerging Threats)
Suricata cần cơ sở dữ liệu các mẫu mã độc. Chạy lệnh sau để tải tệp luật mới nhất:

```bash
Invoke-WebRequest -Uri "[https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules](https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules)" -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"

```

### 3.3 Cấu hình File suricata.yaml

1. Mở file C:\Users\Admin\MiniSIEM\suricata\suricata.yaml bằng Notepad.
2. Tìm dòng default-log-dir: và cập nhật thành đường dẫn sau (Bắt buộc dùng dấu gạch chéo xuôi / để tránh lỗi cú pháp):

default-log-dir: "C:/Users/Admin/MiniSIEM/suricata/log"

3. Lưu và đóng file.

### 3.4 Cấu hình Wazuh Agent liên kết với Suricata
Wazuh Agent (sau khi được cài từ giao diện Web Wazuh) cần được cấu hình để đọc log của Suricata.

1. Mở Notepad dưới quyền Administrator.

2. Mở file C:\Program Files (x86)\ossec-agent\ossec.conf.

3. Bổ sung đoạn cấu hình sau vào trong file để Agent thu thập luồng dữ liệu mạng:\


<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>

Khởi động lại dịch vụ Agent trong PowerShell:

```bash
Restart-Service -Name WazuhSvc

```

## Phần 4: Vận Hành Hệ Thống & Giả Lập Tấn Công


Quy trình kích hoạt hệ thống và demo bắt các cuộc tấn công mạng thực tế.

### 4.1 Khởi chạy Cảm biến Suricata
Trên máy Windows (Nạn nhân), mở PowerShell (Admin) và khởi động Suricata ở chế độ IDS. Thay \Device\NPF_{...} bằng ID Card mạng của bạn:

```bash
cd "C:\Users\Admin\MiniSIEM\suricata"
.\suricata.exe -c suricata.yaml -i "ip-của-bạn" -S suricata.rules -vv

```

(Trạng thái sẵn sàng khi terminal báo cáo: Info: detect: 50208 signatures processed)


### 4.2 Kịch Bản Demo: Phát hiện HTTP Chứa Mã Độc (BlackSun)
Kiểm tra khả năng bắt chữ ký mã độc. Mở một cửa sổ PowerShell mới trên máy Windows và mô phỏng việc máy bị nhiễm Trojan cố gắng kết nối ra ngoài:

```bash
curl.exe -H "User-Agent: BlackSun" [http://testmynids.org](http://testmynids.org)
```

Kết quả hiển thị: Suricata ghi nhận Severity 1, Wazuh Dashboard cảnh báo ở mức Level 12 (Critical/High).

curl.exe -4 -A "BlackSun" http://testmynids.org/uid/index.html?test=ipv4