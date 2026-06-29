# Nhật ký tuần 4 — Báo cáo thực tập

**Ngày:** Tháng 6, 2026  
**Địa điểm:** Làm việc tại Cửa Hàng Vi Tính DuLi    
**Người hướng dẫn:** Châu Văn Mau, Chủ cửa hàng  

---

## Công việc đã thực hiện trong tuần

Tuần này em xây dựng Use Case Diagram, viết Requirement
Specification đầy đủ cho 10 use case, và xác định tiêu chuẩn
đánh giá sản phẩm cuối kỳ.

## Những gì đã học được

- Cách xây dựng Use Case Diagram theo chuẩn UML
- Phân biệt actor "Người dùng" và actor "Hệ thống"
- Tầm quan trọng của tiêu chuẩn đánh giá rõ ràng trước khi
  bắt đầu triển khai

## Khó khăn gặp phải

Việc mô tả các use case liên quan đến Zeek và Grafana phức tạp
hơn dự kiến vì đây là các công cụ bên thứ ba — ranh giới giữa
"hệ thống làm" và "công cụ làm" cần được xác định rõ. Quyết
định: coi toàn bộ stack (Zeek + Promtail + Loki + Grafana) là
"hệ thống MiniSIEM", người dùng chỉ tương tác với Grafana.

## Kế hoạch tuần tiếp theo

- Thiết kế kiến trúc hệ thống chi tiết
- Thiết kế data flow diagram
- Tạo mockup UI cho các Grafana dashboard

---

## Minh chứng tuần 4

| Minh chứng | File |
|---|---|
| Use Case Diagram | evidence/week04/usecase_diagram.png |
| Requirement Specification | evidence/week04/requirement_spec.png |
| Tiêu chuẩn đánh giá | evidence/week04/evaluation_criteria.png |