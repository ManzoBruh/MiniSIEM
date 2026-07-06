# Nhật ký tuần 5 — Báo cáo thực tập

**Ngày:** Tháng 7, 2026  
**Địa điểm:** Làm việc tại Cửa Hàng Vi Tính DuLi    
**Người hướng dẫn:** Châu Văn Mau, Chủ cửa hàng  

---

## Công việc đã thực hiện trong tuần

Tuần này em thiết kế kiến trúc hệ thống chi tiết, vẽ data flow
diagram, và tạo mockup cho các Grafana dashboard. Đây là tuần
quan trọng nhất trong giai đoạn thiết kế — mọi quyết định kỹ
thuật đều được xác định ở đây.

## Kiến trúc hệ thống

Sau khi phân tích kỹ, em quyết định thiết kế MiniSIEM theo
kiến trúc pipeline với 4 layer rõ ràng:

**Layer 1 — Collection:** Zeek lắng nghe traffic mạng và tạo
structured logs dạng JSON

**Layer 2 — Shipping:** Promtail theo dõi log files và đẩy
vào Loki với labels phù hợp

**Layer 3 — Storage:** Loki lưu trữ log với index tối thiểu,
tiết kiệm tài nguyên

**Layer 4 — Visualization:** Grafana truy vấn Loki qua LogQL
và hiển thị dashboard real-time

## Thiết kế Grafana dashboards

Em thiết kế 3 dashboard chính:

**1. Overview Dashboard**
- Tổng số kết nối trong 24h
- Biểu đồ traffic theo thời gian
- Top 10 IP kết nối nhiều nhất
- Số lượng alerts theo mức độ

**2. Network Dashboard**
- Bảng chi tiết tất cả kết nối (conn.log)
- DNS queries timeline
- HTTP traffic table
- Protocol distribution chart

**3. Security Dashboard**
- Alerts từ notice.log
- Weird events từ weird.log
- Port scan detection
- DNS tunneling indicators

## Những gì đã học được

- Cách thiết kế Grafana dashboard hiệu quả cho security use case
- LogQL syntax cho việc extract metrics từ log
- Tầm quan trọng của việc chọn đúng Grafana panel type cho
  từng loại dữ liệu

## Khó khăn gặp phải

Thiết kế mockup cho Grafana dashboard khó hơn dự kiến vì
Grafana có rất nhiều loại panel và cấu hình. Em quyết định
vẽ mockup đơn giản trước, chi tiết cụ thể sẽ được điều chỉnh
trong quá trình triển khai thực tế.

## Kế hoạch tuần tiếp theo

- Cài đặt Docker Desktop
- Thiết lập môi trường phát triển
- Tạo docker-compose.yml cơ bản
- Test từng service riêng lẻ

---

## Minh chứng tuần 5

| Minh chứng | File |
|---|---|
| Architecture Diagram | evidence/week05/architecture.png |
| Data Flow Diagram | evidence/week05/dataflow.png |
| Dashboard Mockup | evidence/week05/dashboard_mockup.png |