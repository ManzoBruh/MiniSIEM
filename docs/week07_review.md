# Báo cáo giữa kỳ — Tuần 7

## 1. Tổng hợp yêu cầu

Hệ thống MiniSIEM cần thực hiện 10 chức năng chính (F01-F10)
bao gồm: Wazuh Agent deployment, log collection real-time,
alerting tự động, Network IDS bằng Suricata, dashboard tập
trung, agent management, vulnerability detection, FIM,
pcap analysis và one-command deployment.

## 2. Ràng buộc

| Loại | Ràng buộc |
|---|---|
| Phần cứng | RAM server tối thiểu 8GB |
| Phần mềm | Ubuntu Server/VirtualBox bắt buộc cho server |
| Thời gian | Hoàn thành trong 16 tuần |
| Chi phí | Miễn phí hoàn toàn |
| Kỹ thuật | Chỉ dùng công nghệ mã nguồn mở |
| Agent | Cài native trên OS, không dùng Docker |

## 3. Tiêu chuẩn đánh giá

| Tiêu chí | Mức đạt | Mức tốt |
|---|---|---|
| Server deploy | Ubuntu Server | < 3 phút |
| Agent connect | Kết nối Manager | < 30 giây |
| Real-time alert | < 60 giây | < 30 giây |
| Suricata | Port scan | Full ET rules |
| Dashboard | Events + agents | Auto-refresh |
| RAM | < 8GB | < 6GB |
| Docs | README | Video demo |

## 4. Kế hoạch thực hiện

| Tuần | Nội dung |
|---|---|
| 8 | Triển khai Wazuh qua Ubuntu Server, cài Agent |
| 9 | Tích hợp Suricata, cấu hình eve.json → Wazuh |
| 10 | Demo giữa kỳ với mentor |
| 11 | Tối ưu, thêm custom rules, sửa lỗi |
| 12 | Testing đầy đủ |
| 13 | Deploy, user manual, demo video |
| 14 | Thu thập phản hồi |
| 15 | Báo cáo tổng kết |
| 16 | Bảo vệ đồ án |

## 5. Kết luận giai đoạn thiết kế

Toàn bộ giai đoạn phân tích và thiết kế (tuần 1-6) đã
hoàn thành. MiniSIEM sử dụng Wazuh + Suricata — giải quyết được vấn đề cốt lõi mà stack Zeek+Loki+Grafana không làm được: agent-based monitoring và real-time alerting trên Windows. Dự án sẵn sàng bước vào giai đoạn triển khai.

