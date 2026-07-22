# Nhật ký tuần 2 — Báo cáo thực tập

**Ngày:** Tháng 6, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Nghiên cứu kiến trúc Wazuh: Manager, Indexer, Dashboard, Agent
- Nghiên cứu Suricata: signature-based detection, EVE JSON output
- Đọc tài liệu chính thức Wazuh và Suricata
- So sánh Wazuh+Suricata với các giải pháp khác
- Tìm hiểu Ubuntu Server deployment cho Wazuh

## Những gì đã học được

- Wazuh Agent là điểm khác biệt cốt lõi — chạy native trên
  endpoint, thu thập log real-time, không cần manual pcap
- Suricata khác Zeek ở chỗ: signature-based detection với
  Emerging Threats rules, hỗ trợ Windows đầy đủ
- Wazuh có hỗ trợ qua Ubuntu — triển khai server
  chỉ cần 3 bước

## Khó khăn gặp phải

Wazuh có nhiều thành phần hơn stack cũ (Zeek+Loki+Grafana),
cần thời gian để hiểu quan hệ giữa Manager, Indexer và Agent.
Sau khi đọc kỹ architecture guide, đã nắm được luồng dữ liệu.

## Kế hoạch tuần tiếp theo

- Làm việc với mentor để thu thập yêu cầu
- Viết user stories
- Xác định phạm vi v1

---

## Minh chứng

| Minh chứng | File |
|---|---|
| Research notes | evidence/week02/wazuh_research.png |
| Suricata research | evidence/week02/suricata_research.png |