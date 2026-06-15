# Nghiên cứu kỹ thuật — Tuần 2

## 1. Tổng quan kiến trúc MiniSIEM

MiniSIEM được xây dựng theo mô hình pipeline một chiều:

[Mạng] → [Zeek] → [Log files] → [Promtail] → [Loki] → [Grafana]


Mỗi thành phần có trách nhiệm riêng biệt và độc lập với nhau,
cho phép thay thế hoặc nâng cấp từng phần mà không ảnh hưởng
đến toàn bộ hệ thống.

## 2. Zeek — Network IDS

### Cách hoạt động
Zeek lắng nghe trên một network interface (hoặc đọc file pcap)
và phân tích từng gói tin theo từng layer. Thay vì chỉ so khớp
signature, Zeek xây dựng "connection state" cho mỗi kết nối và
tạo ra log có cấu trúc khi kết nối kết thúc.

### Các log file quan trọng

| Log file | Nội dung |
|---|---|
| conn.log | Mọi kết nối TCP/UDP/ICMP |
| dns.log | DNS queries và responses |
| http.log | HTTP requests và responses |
| ssl.log | SSL/TLS handshakes |
| notice.log | Cảnh báo bảo mật tự động |
| weird.log | Hành vi mạng bất thường |
| files.log | File transfers qua mạng |

### Format log (JSON)
```json
{
  "ts": 1718000000.0,
  "uid": "CXjN7h3TH6YQ1lZSh",
  "id.orig_h": "192.168.1.100",
  "id.orig_p": 54321,
  "id.resp_h": "8.8.8.8",
  "id.resp_p": 53,
  "proto": "udp",
  "service": "dns",
  "duration": 0.002,
  "orig_bytes": 45,
  "resp_bytes": 120,
  "conn_state": "SF"
}
```

## 3. Promtail — Log Shipper

### Cách hoạt động
Promtail theo dõi (tail) các file log của Zeek theo thời gian
thực. Mỗi dòng log mới được gắn nhãn (label) và đẩy vào Loki
qua HTTP API.

### Cấu hình cơ bản
```yaml
server:
  http_listen_port: 9080

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: zeek
    static_configs:
      - targets: ['localhost']
        labels:
          job: zeek
          __path__: /zeek/logs/current/*.log
```

## 4. Grafana Loki — Log Database

### Tại sao chọn Loki thay vì Elasticsearch

| Tiêu chí | Loki | Elasticsearch |
|---|---|---|
| Index | Chỉ index labels | Index toàn bộ content |
| RAM usage | ~100MB | 2GB+ |
| Storage | Compressed chunks | Large inverted index |
| Query language | LogQL | Lucene/KQL |
| Phù hợp | Log storage | Full-text search |

### LogQL cơ bản
```logql
# Lấy tất cả log từ Zeek
{job="zeek"}

# Lọc DNS queries đến 8.8.8.8
{job="zeek", log_type="dns"} |= "8.8.8.8"

# Đếm số kết nối mỗi phút
count_over_time({job="zeek", log_type="conn"}[1m])
```

## 5. Grafana — Visualization

### Các panel type sẽ sử dụng

| Panel | Dùng để hiển thị |
|---|---|
| Time series | Lưu lượng mạng theo thời gian |
| Table | Danh sách kết nối, DNS queries |
| Stat | Tổng số alerts, kết nối |
| Logs | Raw log viewer |
| Geomap | Phân bố địa lý IP nguồn |

## 6. Docker Compose — Deployment

### Lý do chọn Docker Compose
- Toàn bộ stack chạy bằng 1 câu lệnh
- Môi trường nhất quán giữa dev và production
- Dễ dàng chia sẻ và reproduce
- Không cần cài đặt thủ công từng tool

### Services sẽ định nghĩa
```yaml
services:
  zeek:      # Network IDS
  promtail:  # Log shipper
  loki:      # Log database
  grafana:   # Dashboard
```

## 7. Kết luận nghiên cứu

Stack Zeek + Promtail + Loki + Grafana là lựa chọn tối ưu cho
MiniSIEM vì:
- Tất cả đều miễn phí và mã nguồn mở
- Tổng RAM sử dụng ước tính dưới 512MB
- Có thể chạy trên máy tính cá nhân bình thường
- Được sử dụng trong môi trường production thực tế
- Cộng đồng hỗ trợ lớn, tài liệu đầy đủ