🛡️ MiniSIEM — Hệ Thống Giám Sát An Toàn Thông Tin Tinh Gọn

MiniSIEM là một hệ thống giám sát an toàn thông tin (SIEM) phân tán mã nguồn mở, được tối ưu hóa cho các môi trường cá nhân, phòng lab hoặc doanh nghiệp quy mô nhỏ.

Hệ thống kết hợp sức mạnh phân tích log tập trung của Wazuh và khả năng phát hiện xâm nhập mạng (IDS) theo thời gian thực của Suricata Engine, giúp phát hiện kịp thời các mối đe dọa như quét cổng (Port Scanning), mã độc, hay truy cập Web bất thường.

🏗️ Kiến Trúc Hệ Thống

Hệ thống được triển khai theo mô hình phân tán (Client-Server) trên môi trường ảo hóa:

[ Lưu Lượng Mạng / Kẻ Tấn Công ]
             ↓
[ Windows Endpoint (Máy Nạn Nhân) ]
  ├── Suricata IDS (Phân tích gói tin real-time)
  │     └─ Ghi log cảnh báo → C:\...\suricata\log\eve.json
  └── Wazuh Agent (Theo dõi file & Chuyển tiếp log an toàn)
             ↓  (Gửi log qua Port 1514 - Bridged Network)
[ Ubuntu Server 24.04 (Wazuh Manager) ]
  ├── Wazuh Indexer (Lưu trữ và đánh chỉ mục)
  ├── Wazuh Server (Chấm điểm Severity & So khớp tập luật)
  └── Wazuh Dashboard (Giao diện hiển thị trực quan)
             ↓
[ Quản Trị Viên (Web Browser - HTTPS) ]


🧰 Tech Stack

Tầng / Vai Trò

Công Nghệ

Mô Tả & Lý Do Lựa Chọn

Máy Chủ Phân Tích

Ubuntu Server 24.04

Nền tảng Linux mã nguồn mở, tối ưu tài nguyên cho Server.

SIEM & XDR Core

Wazuh Manager 4.x

Thu thập log tập trung, phân tích, và cung cấp OpenSearch Dashboard.

Network IDS

Suricata 7.0 (Windows)

Động cơ kiểm tra sâu gói tin mạng (DPI), kết hợp tập luật Emerging Threats.

Log Collector

Wazuh Agent

Dịch vụ siêu nhẹ chạy ngầm, tự động đẩy log eve.json về trung tâm.

Môi Trường

Oracle VirtualBox

Cung cấp mạng Bridged để giả lập kết nối như phần cứng vật lý.

⚡ Cài Đặt & Vận Hành Nhanh

1. Cài đặt Trung Tâm Giám Sát (Ubuntu Server)

Sử dụng script tự động của Wazuh:

curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a


(Ghi lại Username và Password được cấp cuối quá trình cài đặt).

2. Cài đặt Cảm Biến (Windows Endpoint)

Tạo thư mục log: Chạy PowerShell quyền Admin: New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"

Cập nhật suricata.yaml: Đổi default-log-dir trỏ về thư mục vừa tạo (dùng dấu /).

Tải luật (Rules):

Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules" -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"


Cấu hình Agent: Bổ sung block sau vào C:\Program Files (x86)\ossec-agent\ossec.conf để agent đọc log mạng:

<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>


Khởi động lại Agent: Restart-Service -Name WazuhSvc

🚀 Kịch Bản Kiểm Thử (Demo Tấn Công)

Đầu tiên, khởi chạy Suricata trên máy Windows (Nạn nhân):

cd "C:\Users\Admin\MiniSIEM\suricata"
.\suricata.exe -c suricata.yaml -i "\Device\NPF_{YOUR_ADAPTER_ID}" -S suricata.rules -vv


Sau đó, thực hiện các kịch bản tấn công:

Kịch bản 1: Giả lập Botnet rà quét lỗ hổng
(Mở PowerShell mới, ép dùng IPv4)

curl.exe -4 -A "ZmEu" http://testmynids.org?botnet=1


👉 Kết quả: Dashboard ngay lập tức cảnh báo mức độ Critical/High.

Kịch bản 2: Tấn công Dò quét Cổng (Port Scanning)
(Từ máy Attacker)

1..100 | % { echo ((New-Object Net.Sockets.TcpClient).Connect("IP_NẠN_NHÂN", $_)) "Port $_ is open!" } 2>$null


👉 Kết quả: Xuất hiện hàng loạt log Attempted Recon hiển thị IP kẻ tấn công.

📊 Tùy Chỉnh Bảng Điều Khiển (Custom Dashboards)

Dự án đã xây dựng các Visualization riêng biệt trên nền tảng OpenSearch Dashboards để biến Wazuh thành một SOC (Security Operations Center) thực thụ:

The Threat Landscape (Pie Chart): Phân tích tỷ trọng các cấp độ cảnh báo (Rule Levels).

Top Attack Vectors (Bar Chart): Thống kê các loại hình tấn công phổ biến nhất dựa trên chữ ký Suricata.

Attacker IPs (Data Table): Bảng xếp hạng và trích xuất trực tiếp địa chỉ IP của các tác nhân độc hại.

📁 Cấu Trúc Thư Mục

MiniSIEM/
├── docs/                        # Tài liệu hệ thống & Báo cáo
│   └── User_Guide_VN.md         # Hướng dẫn sử dụng & Fix lỗi
├── suricata/
│   ├── suricata.yaml            # Cấu hình Engine mạng
│   └── suricata.rules           # (Ignored) Tập luật tải động
├── wazuh/
│   └── ossec.conf               # File cấu hình mẫu cho Agent
├── .gitignore                   # Loại bỏ file .exe và log cục bộ
└── README.md                    # Tài liệu dự án


🎯 Chức Năng Cốt Lõi

✅ Tích hợp thành công Suricata IDS vào kiến trúc Wazuh Manager.

✅ Thu thập, giải mã và phân tích luồng JSON (eve.json) theo thời gian thực.

✅ Phát hiện mạnh mẽ các kỹ thuật Scanning, Botnet, Web Vulnerability qua Ruleset chuẩn.

✅ Lọc dữ liệu linh hoạt trên OpenSearch và quản lý Dashboard tập trung.

✅ Tối ưu hóa bộ nhớ cho Endpoint (Mức tiêu thụ CPU/RAM thấp khi chạy nền).

Dự án thực tập tại Cửa Hàng Vi Tính DuLi

Người hướng dẫn: Châu Văn Mau

Sinh viên thực hiện: ManzoBruh
