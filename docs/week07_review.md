# Báo cáo giữa kỳ — Tuần 7

## 1. Tổng hợp yêu cầu

### Yêu cầu chức năng (F01-F10)
Hệ thống MiniSIEM cần thực hiện 10 chức năng chính bao gồm
giám sát mạng real-time, thu thập và lưu trữ log, hiển thị
dashboard, tìm kiếm log, phân tích pcap, và triển khai
bằng một câu lệnh.

### Yêu cầu phi chức năng
Hệ thống phải hoạt động trên máy cá nhân thông thường với
RAM dưới 512MB, độ trễ dashboard dưới 5 giây, tương thích
đa nền tảng, và hoàn toàn mã nguồn mở.

## 2. Ràng buộc

| Loại | Ràng buộc |
|---|---|
| Phần cứng | Máy tính cá nhân, RAM tối thiểu 4GB |
| Phần mềm | Docker Desktop bắt buộc |
| Thời gian | Hoàn thành trong 16 tuần |
| Chi phí | Miễn phí hoàn toàn |
| Kỹ thuật | Chỉ dùng công nghệ mã nguồn mở |

## 3. Tiêu chuẩn đánh giá

| Tiêu chí | Mức đạt | Mức tốt |
|---|---|---|
| Deploy | docker compose up hoạt động | Chạy < 60 giây |
| Zeek | Parse pcap file | Real-time capture |
| Loki | Nhận và lưu log | Query < 2 giây |
| Grafana | 3 dashboard | Auto-refresh 5s |
| RAM | < 512MB | < 256MB |
| Docs | README đầy đủ | Video demo |

## 4. Kế hoạch thực hiện

| Tuần | Nội dung |
|---|---|
| 8 | Docker Compose stack, Zeek config, Loki config |
| 9 | Promtail config, Grafana datasource, dashboard cơ bản |
| 10 | Demo giữa kỳ với mentor |
| 11 | Tối ưu, thêm dashboard nâng cao, Zeek custom scripts |
| 12 | Testing đầy đủ |
| 13 | Deploy, user manual, demo video |
| 14 | Thu thập phản hồi |
| 15 | Báo cáo tổng kết |
| 16 | Bảo vệ đồ án |

## 5. Kết luận giai đoạn thiết kế

Toàn bộ giai đoạn phân tích và thiết kế (tuần 1-6) đã hoàn
thành. Hệ thống MiniSIEM sử dụng stack Zeek + Promtail +
Loki + Grafana, triển khai bằng Docker Compose, đáp ứng
đầy đủ yêu cầu về tính đơn giản, hiệu năng và chi phí.
Dự án sẵn sàng bước vào giai đoạn triển khai từ tuần 8.