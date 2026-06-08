# 🛡️ MiniSIEM — Hệ thống giám sát bảo mật mạng cá nhân

MiniSIEM là một hệ thống giám sát bảo mật mạng cá nhân
mã nguồn mở, được xây dựng trên nền tảng các công nghệ
đã được kiểm chứng trong môi trường doanh nghiệp.

Mục tiêu: mang lại khả năng giám sát bảo mật chuyên nghiệp
đến tay người dùng cá nhân — miễn phí, nhẹ, và triển khai
chỉ bằng một câu lệnh.

---

## 🏗️ Kiến trúc hệ thống

[Traffic mạng / File .pcap]
↓
[Zeek — Network IDS]
Phân tích gói tin, tạo structured logs JSON
↓
[Promtail — Log Shipper]
Thu thập log, gắn labels, đẩy vào Loki
↓
[Grafana Loki — Log Database]
Lưu trữ log tối ưu, index theo labels
↓
[Grafana — Visualization]
Dashboard real-time, truy vấn LogQL
↓
[Người dùng — Browser]
localhost:3000

---

## 🧰 Tech stack

| Tầng | Công nghệ | Lý do |
|---|---|---|
| Network IDS | Zeek | Phân rã dữ liệu mạng chi tiết dạng JSON, rất nhẹ |
| Log Shipper | Promtail | Thu thập và chuyển tiếp log thời gian thực |
| Log Database | Grafana Loki | Lưu trữ log tối ưu, không index toàn bộ dữ liệu |
| Visualization | Grafana | Dashboard giám sát real-time, truy vấn LogQL |
| Triển khai | Docker Compose | Cô lập môi trường, chạy hệ thống bằng 1 câu lệnh |
| Quản lý mã nguồn | Git + GitHub | Chuẩn công nghiệp |

---

## ⚡ Quick Start

### Yêu cầu
- Docker Desktop 4.0+
- RAM tối thiểu 4GB
- Windows 10/11 (WSL2), Linux, hoặc macOS

### Cài đặt

```bash
# 1. Clone repository
git clone https://github.com/ManzoBruh/MiniSIEM.git
cd MiniSIEM

# 2. Tạo file cấu hình
cp .env.example .env

# 3. Khởi động hệ thống
docker compose up -d

# 4. Mở dashboard
# Truy cập http://localhost:3000
# Username: admin
# Password: xem file .env
```

### Phân tích file pcap

```bash
# Chạy Zeek với file pcap của bạn
docker exec minisiem-zeek zeek -r /samples/your-file.pcap local

# Log sẽ tự động xuất hiện trên Grafana
```

---

## 📊 Dashboards

| Dashboard | Mô tả |
|---|---|
| Overview | Tổng quan traffic, tổng số kết nối, alerts |
| Network | Chi tiết kết nối, DNS queries, HTTP traffic |
| Security | Cảnh báo Zeek, anomalies, port scan detection |

---

## 📁 Cấu trúc thư mục

MiniSIEM/
├── docker-compose.yml       # Stack deployment
├── .env.example             # Config template
├── zeek/config/             # Zeek configuration
├── promtail/config/         # Promtail configuration
├── loki/config/             # Loki configuration
├── grafana/                 # Grafana config + dashboards
├── samples/                 # Sample pcap files
├── docs/                    # Documentation + diary
└── tests/                   # Test cases


---

## 🎯 Tính năng

- ✅ Phân tích file `.pcap` với Zeek
- ✅ Giám sát mạng real-time
- ✅ Dashboard tổng quan lưu lượng mạng
- ✅ Phát hiện port scan tự động
- ✅ Phát hiện DNS tunneling
- ✅ Tìm kiếm log bằng LogQL
- ✅ Triển khai bằng một câu lệnh
- ✅ RAM sử dụng dưới 350MB

---

## 📖 Tài liệu

- [Nhật ký thực tập 16 tuần](docs/diary/)
- [Tài liệu kỹ thuật](docs/)
- [Kết quả kiểm thử](tests/)

---

## 🔮 Hướng phát triển (v2)

- Tích hợp AbuseIPDB threat intelligence
- Email/Telegram alerting
- Mobile dashboard
- Machine learning based anomaly detection
- Wazuh integration

---

## 📄 License

MIT License — xem [LICENSE](LICENSE)

---

> Dự án thực tập tại Cửa Hàng Vi Tính DuLi  
> Người hướng dẫn: Châu Văn Mau  
> Sinh viên: ManzoBruh