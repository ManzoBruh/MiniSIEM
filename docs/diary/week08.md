# Nhật ký tuần 8 — Báo cáo thực tập

**Ngày:** Tháng 7, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Generate TLS certificates cho Wazuh stack
- Triển khai Wazuh Server: Manager + Indexer + Dashboard
- Cài Wazuh Agent lần đầu trên Windows 11
- Xác nhận Agent kết nối thành công về Manager
- Kiểm tra Windows Event Log được thu thập

## Kết quả đáng chú ý

Sau khi cài Agent và tạo một số failed login attempts,
alerts xuất hiện trên Wazuh Dashboard trong vòng ~25 giây.
Đây là lần đầu tiên thấy hệ thống SIEM real-time hoạt động.

## Những gì đã học được

- Quy trình generate Wazuh TLS certificates
- Cách Wazuh Agent enrollment: tự động qua Manager API
- Wazuh rule levels: 1-15, level 7+ là alerts đáng chú ý
- Windows Event ID mapping với Wazuh rules

## Khó khăn gặp phải

Wazuh Dashboard dùng self-signed certificate — Chrome cảnh báo
"Not Secure". Giải pháp: click Advanced → Proceed hoặc
import cert vào trusted store.

## Kế hoạch tuần tiếp theo

- Cài và cấu hình Suricata
- Tích hợp Suricata EVE JSON với Wazuh Agent
- Test phát hiện network attacks

---