# Nhật ký tuần 9 — Báo cáo thực tập

**Ngày:** Tháng 7, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Cài đặt Suricata trên Ubuntu (WSL2)
- Cập nhật Emerging Threats rules (suricata-update)
- Cấu hình Suricata EVE JSON output
- Cấu hình Wazuh Agent đọc /var/log/suricata/eve.json
- Test phân tích file pcap với Suricata
- Xác nhận Suricata alerts xuất hiện trên Wazuh Dashboard

## Kết quả tích hợp

Pipeline hoàn chỉnh:

Suricata → eve.json → Wazuh Agent → Manager → Dashboard

## Phân tích file pcap 2025-01-22:

15+ Attempted Recon alerts
8+ Malware Communication alerts
5+ Suspicious SSL alerts
Tất cả hiển thị trên Wazuh Dashboard với agent info.

## Những gì đã học được

Suricata-update command để sync ET rules
Cấu hình ossec.conf để đọc JSON log format
Wazuh decoder cho Suricata EVE JSON events
Cách Wazuh categorize Suricata alerts theo rule groups

## Khó khăn gặp phải
Wazuh Agent cần restart sau khi thêm localfile config mới.
Suricata EVE JSON timestamp format cần match với Wazuh
decoder expectations.

## Kế hoạch tuần tiếp theo

Demo hệ thống với mentor
Chuẩn bị slide mid-term review

## Minh chứng
| Minh chứng | File |
|---|---|
| Suricata running | evidence/week09/suricata_running.png |
| EVE JSON output | evidence/week09/eve_json.png |
| Suricata alerts Wazuh | evidence/week09/suricata_wazuh.png |
| Pull request | evidence/week09/pull_request.png |