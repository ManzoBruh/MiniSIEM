# Nhật ký tuần 13 — Báo cáo thực tập

**Ngày:** Tháng 8, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Hoàn thiện README với step-by-step installation guide
- Viết User Manual (3 phần: cài đặt, sử dụng, pcap analysis)
- Quay video demo 5 phút
- Final deployment test trên clean machine

## User Manual Structure

**Phần 1 — Quick Start (5-10 phút)**
- Yêu cầu hệ thống
- Cài Docker Desktop
- Clone + deploy Wazuh Server

**Phần 2 — Cài Wazuh Agent**
- Windows step-by-step
- Linux/Ubuntu step-by-step
- Xác nhận kết nối

**Phần 3 — Sử dụng Suricata**
- Cài Suricata
- Update ET rules
- Phân tích pcap
- Xem kết quả trên Wazuh Dashboard

## Video demo script

1. Khởi động
2. Mở Dashboard: https://localhost
3. Show Agents management (3 agents active)
4. Tạo suspicious event → show alert real-time
5. Chạy Suricata với pcap → show network alerts
6. Điều tra attack chain trên Dashboard

## Kế hoạch tuần tiếp theo

- Thu thập phản hồi người dùng
- Đánh giá sản phẩm

---
