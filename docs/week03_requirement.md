# Yêu cầu hệ thống — Tuần 3

## 1. Người dùng mục tiêu

| Nhóm | Mô tả |
|---|---|
| Người dùng cá nhân | Muốn giám sát bảo mật máy tính và mạng gia đình |
| Sinh viên CNTT | Học về SIEM, cần môi trường lab thực hành |
| SOC Analyst cá nhân | Muốn có home lab với công cụ doanh nghiệp |

## 2. User Stories

| ID | User Story |
|---|---|
| US01 | Là người dùng, tôi muốn cài agent trên máy tính để giám sát real-time |
| US02 | Là người dùng, tôi muốn xem tất cả security events từ mọi endpoint trên dashboard |
| US03 | Là người dùng, tôi muốn nhận cảnh báo ngay khi có tấn công mạng |
| US04 | Là người dùng, tôi muốn biết máy tính có lỗ hổng bảo mật nào không |
| US05 | Là người dùng, tôi muốn phát hiện khi file quan trọng bị thay đổi |
| US06 | Là người dùng, tôi muốn xem hệ thống được phát hiện tấn công cổng nào |
| US07 | Là người dùng, tôi muốn phân tích file pcap và xem kết quả trên dashboard |
| US08 | Là người dùng, tôi muốn theo dõi trạng thái kết nối của tất cả agents |
| US09 | Là người dùng, tôi muốn triển khai server nhanh nhất và tối ưu nhất |
| US10 | Là người dùng, tôi muốn xem lịch sử alerts để điều tra sự cố |
| US11 | Là người dùng, tôi muốn có cách nào dễ dàng nhất để hiểu biết thêm về SIEM |


## 3. Yêu cầu chức năng

| ID | Chức năng | Mô tả |
|---|---|---|
| F01 | Wazuh Agent deployment | Cài agent trên endpoint, gửi log real-time |
| F02 | Log collection | Manager nhận và phân tích log từ tất cả agents |
| F03 | Real-time alerting | Tạo cảnh báo ngay khi phát hiện sự kiện bảo mật |
| F04 | Network IDS | Suricata phát hiện tấn công và gửi alerts về Wazuh |
| F05 | Security dashboard | Wazuh Dashboard hiển thị tổng quan security events |
| F06 | Agent management | Quản lý và theo dõi trạng thái tất cả agents |
| F07 | Vulnerability detection | Phát hiện CVE trên các endpoint |
| F08 | File integrity monitoring | Phát hiện thay đổi bất thường trong file hệ thống |
| F09 | pcap analysis | Suricata phân tích file pcap và gửi kết quả về Wazuh |
| F10 | One-command deploy | Server stack chạy bằng curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh –a |

## 4. Yêu cầu phi chức năng

| ID | Yêu cầu | Mô tả |
|---|---|---|
| NF01 | Hiệu năng | Server RAM < 8GB; Agent overhead < 100MB |
| NF02 | Real-time | Alert xuất hiện < 60 giây sau sự kiện |
| NF03 | Scalability | Hỗ trợ tối thiểu 5 agents đồng thời |
| NF04 | Tương thích | Agent chạy trên Windows 10/11 và Ubuntu 22.04 |
| NF05 | Bảo mật | Giao tiếp Agent-Manager mã hóa TLS |
| NF06 | Mã nguồn mở | 100% công nghệ mã nguồn mở |

## 5. Phạm vi v1

### Trong phạm vi
- Triển khai Wazuh Server bằng Ubuntu Server qua VirtualBox
- Wazuh Agent trên Windows và Linux
- Suricata Network IDS tích hợp với Wazuh
- Phân tích file pcap offline bằng Suricata
- Wazuh Dashboard giám sát tập trung
- Real-time alerting

### Ngoài phạm vi (v2)
- Active response tự động block IP
- Threat intelligence integration
- Telegram/email alerting
- Machine learning detection
- MITRE ATT&CK mapping

## 6. Sơ đồ nghiệp vụ

[Endpoint]
↓ Wazuh Agent (real-time)
[Wazuh Manager]
↓ Phân tích + tương quan
[Wazuh Indexer]
↓ Lưu trữ events
[Wazuh Dashboard]
↓ Hiển thị alerts
[Người dùng điều tra]

[Network Traffic]
↓ Suricata listen on interface
[EVE JSON alerts]
↓ Wazuh Agent đọc
[Wazuh Manager]
↓ Hiển thị Network IDS alerts
[Wazuh Dashboard]