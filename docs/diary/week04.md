# Nhật ký tuần 4 — Báo cáo thực tập

**Ngày:** Tháng 6, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Xây dựng Use Case Diagram với 10 use cases
- Viết Requirement Specification chi tiết cho từng UC
- Xác định tiêu chuẩn đánh giá sản phẩm

## Những gì đã học được

- Cách xây dựng Use Case Diagram cho hệ thống agent-based
- Phân biệt rõ actor "Người dùng" (tương tác dashboard,
  cài agent) và "Hệ thống" (Agent thu thập log tự động,
  Suricata detect, Manager tạo alerts)

## Khó khăn gặp phải

Wazuh có nhiều tính năng built-in (FIM, vulnerability scan,
active response) khiến việc xác định phạm vi UC phức tạp.
Quyết định: chỉ document các UC trong phạm vi v1.

## Kế hoạch tuần tiếp theo

- Thiết kế kiến trúc chi tiết
- Thiết kế Wazuh Alert Schema thay ERD
- Tạo mockup Wazuh Dashboard

---

## Minh chứng

| Minh chứng | File |
|---|---|
| Use Case Diagram | evidence/week04/usecase_diagram.png |
| Requirement Spec | evidence/week04/requirement_spec.png |