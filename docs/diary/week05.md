# Nhật ký tuần 5 — Báo cáo thực tập

**Ngày:** Tháng 7, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Thiết kế kiến trúc 4-layer: Agent → Manager → Indexer → Dashboard
- Thiết kế Wazuh Alert Schema và Suricata EVE JSON Schema
- Thiết kế cấu hình Suricata (suricata.yaml)
- Thiết kế cấu hình Wazuh Agent (ossec.conf Suricata integration)
- Vẽ mockup Wazuh Dashboard layout

## Những gì đã học được

- Kiến trúc Wazuh single-node vs multi-node deployment
- Cách Wazuh Agent đọc Suricata EVE JSON và ship về Manager
- Cấu trúc Ubuntu cho Wazuh stack

## Khó khăn gặp phải

Wazuh yêu cầu RAM 8GB+ cho server — cao hơn mục tiêu ban đầu.
Nghiên cứu và xác nhận: đây là yêu cầu tối thiểu của
Wazuh Indexer (OpenSearch). Quyết định điều chỉnh tiêu
chuẩn RAM từ 4GB lên 8GB.

## Kế hoạch tuần tiếp theo

- Cài đặt VirtulBox
- Triển khai Ubuntu Server
- Curl wazuh về server
- Test triển khai cơ bản

---

## Minh chứng

| Minh chứng | File |
|---|---|
| Architecture Diagram | evidence/week05/architecture.png |
| Dashboard Mockup | evidence/week05/mockup.png |