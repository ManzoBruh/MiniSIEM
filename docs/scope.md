# MiniSIEM — Tài liệu phạm vi dự án

## 1. Bối cảnh

Trong môi trường bảo mật thông tin hiện đại, các tổ chức
lớn sử dụng các hệ thống SIEM (Security Information and
Event Management) như Splunk hay IBM QRadar để giám sát
và phát hiện mối đe dọa. Tuy nhiên, chi phí của các giải
pháp này (từ $150/tháng trở lên) khiến chúng hoàn toàn
nằm ngoài tầm với của người dùng cá nhân.

Trong khi đó, môi trường IT cá nhân và hộ gia đình ngày
càng phức tạp — nhiều thiết bị kết nối, nhiều dịch vụ
online, nhiều rủi ro bảo mật — nhưng lại không có công cụ
giám sát phù hợp.

MiniSIEM ra đời để lấp đầy khoảng trống này.

## 2. Mục tiêu dự án

Xây dựng một hệ thống giám sát bảo mật mạng cá nhân:
- **Miễn phí** — hoàn toàn dựa trên công nghệ mã nguồn mở
- **Nhẹ** — chạy được trên máy tính cá nhân thông thường
- **Đơn giản** — triển khai bằng một câu lệnh duy nhất
- **Thực dụng** — giải quyết các mối đe dọa thực tế

## 3. Phạm vi v1

### Trong phạm vi
- Phân tích file `.pcap` bằng Zeek
- Giám sát mạng real-time (khi có quyền admin)
- Thu thập và lưu trữ log bằng Promtail + Loki
- Dashboard tổng quan, mạng, và bảo mật bằng Grafana
- Phát hiện tự động: port scan, DNS tunneling
- Tìm kiếm log bằng LogQL trong Grafana Explore
- Triển khai bằng Docker Compose

### Ngoài phạm vi (v2)
- Tích hợp AbuseIPDB threat intelligence
- Email/SMS/Telegram alerting
- Mobile application
- Cloud deployment
- Machine learning detection
- Wazuh/Suricata integration
- Multi-user authentication

## 4. Tech stack

| Tầng | Công nghệ | Phiên bản |
|---|---|---|
| Network IDS | Zeek | 6.x |
| Log Shipper | Promtail | 2.9.x |
| Log Database | Grafana Loki | 2.9.x |
| Visualization | Grafana | 10.x |
| Deployment | Docker Compose | 2.x |

## 5. Ràng buộc

| Loại | Ràng buộc |
|---|---|
| Chi phí | Miễn phí hoàn toàn |
| Phần cứng | RAM tối thiểu 4GB |
| Phần mềm | Docker Desktop bắt buộc |
| Thời gian | 16 tuần |
| Kỹ thuật | Chỉ dùng công nghệ mã nguồn mở |

## 6. Tiêu chuẩn thành công

| Tiêu chí | Mức đạt | Mức tốt |
|---|---|---|
| Triển khai | docker compose up hoạt động | Khởi động < 60 giây |
| Zeek | Parse được pcap file | Real-time capture |
| Loki | Nhận và lưu log | Query < 2 giây |
| Grafana | 3 dashboard hoạt động | Auto-refresh ≤ 5 giây |
| RAM | < 512MB | < 350MB |
| Tài liệu | README đầy đủ | Video demo 5 phút |