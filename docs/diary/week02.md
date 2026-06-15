# Nhật ký tuần 2 — Báo cáo thực tập

**Ngày:** Tháng 6, 2026  
**Địa điểm:** Làm việc tại Cửa Hàng Vi Tính DuLi    
**Người hướng dẫn:** Châu Văn Mau, Chủ cửa hàng  

---

## Công việc đã thực hiện trong tuần

Tuần này em tập trung hoàn toàn vào việc nghiên cứu các công
nghệ sẽ được sử dụng trong dự án MiniSIEM. Mục tiêu là hiểu
rõ cách hoạt động của từng thành phần trước khi bắt đầu thiết
kế và triển khai.

## Kết quả nghiên cứu

### Zeek (Network IDS)
Zeek là một framework phân tích lưu lượng mạng mã nguồn mở,
được phát triển bởi Vern Paxson tại ICSI/Berkeley. Khác với
các IDS truyền thống như Snort hay Suricata vốn hoạt động theo
cơ chế signature-based (so khớp với danh sách mẫu tấn công đã
biết), Zeek tiếp cận theo hướng behavioral analysis — phân tích
hành vi mạng và tạo ra các log có cấu trúc chi tiết cho mọi
kết nối, DNS query, HTTP request, SSL handshake, v.v.

Output của Zeek là các file log dạng JSON hoặc TSV, bao gồm:
- `conn.log` — toàn bộ kết nối mạng
- `dns.log` — DNS queries và responses
- `http.log` — HTTP traffic
- `ssl.log` — SSL/TLS handshakes
- `notice.log` — các cảnh báo bảo mật
- `weird.log` — các hành vi bất thường

### Promtail (Log Shipper)
Promtail là một agent thu thập log được phát triển bởi Grafana
Labs, thiết kế chuyên biệt để đẩy log vào Grafana Loki. Promtail
hoạt động bằng cách theo dõi (tail) các file log theo thời gian
thực, gắn nhãn (label) cho từng dòng log, và chuyển tiếp vào
Loki qua HTTP.

Ưu điểm chính:
- Tốn rất ít tài nguyên CPU và RAM
- Hỗ trợ pipeline để parse và transform log trước khi gửi
- Cấu hình đơn giản bằng YAML

### Grafana Loki (Log Database)
Loki là một hệ thống lưu trữ log được thiết kế theo triết lý
"like Prometheus, but for logs" — chỉ index metadata (labels)
thay vì index toàn bộ nội dung log. Điều này giúp Loki tiêu
thụ ít tài nguyên hơn rất nhiều so với Elasticsearch.

Loki sử dụng ngôn ngữ truy vấn LogQL cho phép:
- Lọc log theo labels
- Thực hiện aggregation và metric extraction
- Tích hợp trực tiếp với Grafana

### Grafana (Visualization)
Grafana là nền tảng visualization mã nguồn mở phổ biến nhất
trong lĩnh vực monitoring và observability. Grafana hỗ trợ
hàng chục datasource khác nhau, trong đó có Loki.

Với Grafana, em có thể:
- Tạo dashboard real-time theo dõi lưu lượng mạng
- Thiết lập alert rule khi phát hiện hành vi bất thường
- Visualize log data dưới dạng bảng, biểu đồ, heatmap

### Docker Compose (Deployment)
Docker Compose cho phép định nghĩa và chạy toàn bộ stack
(Zeek + Promtail + Loki + Grafana) bằng một file YAML duy nhất
và một câu lệnh `docker compose up`. Mỗi service chạy trong
container riêng biệt, cô lập với nhau và với hệ thống host.

## So sánh với các giải pháp thay thế

| Tiêu chí | MiniSIEM Stack | Splunk | ELK Stack |
|---|---|---|---|
| Chi phí | Miễn phí | $150+/tháng | Miễn phí (tự host) |
| Tài nguyên | Rất nhẹ | Nặng | Nặng |
| Độ phức tạp | Vừa phải | Cao | Cao |
| Phù hợp cá nhân | ✅ | ❌ | ⚠️ |
| Real-time | ✅ | ✅ | ✅ |

## Những gì đã học được

- Hiểu sự khác biệt giữa signature-based IDS và behavioral
  analysis IDS — Zeek thuộc nhóm thứ hai, phù hợp hơn cho
  việc phân tích log sau sự cố
- Hiểu tại sao Loki hiệu quả hơn Elasticsearch cho use case
  này: không cần full-text index, chỉ cần tìm kiếm theo label
- Nắm được luồng dữ liệu tổng thể:
  Network traffic → Zeek → logs → Promtail → Loki → Grafana

## Khó khăn gặp phải

Tài liệu của Zeek khá phức tạp, đặc biệt phần scripting language
của Zeek (Zeek Script) có cú pháp riêng khá khác so với các
ngôn ngữ lập trình thông thường. Em quyết định tập trung vào
việc sử dụng Zeek với cấu hình mặc định trước, sau đó mới
nghiên cứu tùy chỉnh ở các tuần sau.

## Kế hoạch tuần tiếp theo

- Làm việc với mentor để thu thập yêu cầu hệ thống
- Viết user stories cho các tính năng cốt lõi
- Xác định phạm vi v1 của dự án
- Vẽ sơ đồ nghiệp vụ

---

## Minh chứng tuần 2

| Minh chứng | File |
|---|---|
| Tài liệu nghiên cứu Zeek | evidence/week02/zeek_research.png |
| Tài liệu nghiên cứu Grafana | evidence/week02/grafana_research.png |
| Nhật ký thực tập | evidence/week02/diary.png |