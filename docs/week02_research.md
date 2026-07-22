# Nghiên cứu kỹ thuật — Tuần 2

## 1. Tổng quan kiến trúc MiniSIEM

MiniSIEM được xây dựng theo mô hình agent-based SIEM:

[Endpoint + Wazuh Agent] → [Wazuh Manager] → [Wazuh Indexer] → [Wazuh Dashboard]
↑
[Suricata] → eve.json → [Wazuh Agent]

## 2. Wazuh — Open Source SIEM Platform

### Lịch sử và tổng quan
Wazuh được fork từ OSSEC năm 2015 và hiện là một trong những
nền tảng SIEM mã nguồn mở phổ biến nhất thế giới với hơn
10 triệu lượt download. Wazuh được sử dụng rộng rãi trong
các doanh nghiệp, tổ chức chính phủ và môi trường học thuật.

### Kiến trúc 3 thành phần

| Thành phần | Chức năng |
|---|---|
| Wazuh Agent | Chạy trên endpoint, thu thập log, FIM, vulnerability scan |
| Wazuh Manager | Nhận log từ agents, phân tích, tương quan, tạo alerts |
| Wazuh Indexer | Lưu trữ events (OpenSearch/Elasticsearch) |
| Wazuh Dashboard | Giao diện web visualization |

### Tính năng cốt lõi

**Log Analysis:**
Agent thu thập log từ Windows Event Log, syslog, ứng dụng
và gửi về Manager. Manager áp dụng rules để phát hiện
hành vi đáng ngờ và tạo alerts với severity level 1-15.

**File Integrity Monitoring (FIM):**
Giám sát thay đổi bất thường trong file hệ thống —
phát hiện khi file bị tạo, sửa, xóa trái phép.

**Vulnerability Detection:**
Tự động scan và báo cáo CVE trên các package đã cài đặt.

**Active Response:**
Tự động block IP, kill process khi phát hiện tấn công.

## 3. Suricata — Network IDS/IPS

### Tổng quan
Suricata được phát triển bởi OISF (Open Information Security
Foundation) từ năm 2009. Khác với Zeek (behavioral analysis),
Suricata sử dụng signature-based detection — so khớp traffic
với database rules (Emerging Threats) để phát hiện tấn công.

### Tại sao chọn Suricata thay vì Zeek

| Tiêu chí | Suricata | Zeek |
|---|---|---|
| Windows support | ✓ | ✗ (dropped) |
| Wazuh integration | ✓ Native | Partial |
| Signature detection | ✓ | ✗ |
| Emerging Threats rules | ✓ | ✗ |
| Real-time alerting | ✓ | Log-based only |

### EVE JSON Output
Suricata ghi alerts ra file eve.json với cấu trúc:
````json
{
  "timestamp": "2025-01-22T14:32:01",
  "event_type": "alert",
  "src_ip": "10.1.17.215",
  "dest_ip": "192.168.1.1",
  "proto": "TCP",
  "alert": {
    "signature": "ET SCAN Nmap Scripting Engine",
    "severity": 2,
    "category": "Attempted Recon"
  }
}
````

## 4. Ubuntu Server LTS — Deployment

Wazuh cung cấp hỗ trợ tối ưu cho nền tảng linux, cho phép
triển khai toàn bộ server stack (Manager + Indexer +
Dashboard) bằng một câu lệnh:

````bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh –a
````

Wazuh Agent được cài native trên OS — không dùng Docker.

## 5. So sánh với giải pháp cũ (Zeek + Loki + Grafana)

| Tiêu chí | Zeek+Loki+Grafana | Wazuh+Suricata |
|---|---|---|
| Agent-based | ✗ | ✓ |
| Real-time alerting | Cần cấu hình | ✓ Built-in |
| Windows native | ✗ (Zeek) | ✓ (Agent + Suricata) |
| Endpoint monitoring | ✗ | ✓ |
| Vulnerability scan | ✗ | ✓ |
| FIM | ✗ | ✓ |
| Phức tạp hơn | Ít | Trung bình |

## 6. Kết luận nghiên cứu

Wazuh + Suricata là stack tối ưu cho MiniSIEM vì:
- Wazuh Agent chạy native trên Windows — giải quyết
  được vấn đề live monitoring mà Zeek không làm được
- Suricata hỗ trợ Windows đầy đủ với Npcap
- Wazuh có real-time alerting built-in
- Toàn bộ miễn phí và mã nguồn mở
- Được dùng trong môi trường production thực tế