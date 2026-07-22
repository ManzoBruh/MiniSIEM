# Nhật ký tuần 12 — Báo cáo thực tập

**Ngày:** Tháng 8, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

Thực hiện kiểm thử đầy đủ theo 3 loại:
Functional, Security và Performance Testing.

## Functional Testing

| Test | Kết quả |
|---|---|
| TC01: Ubuntu Setup | ✓ Pass |
| TC02: Dashboard truy cập | ✓ Pass |
| TC03: Agent Windows connect | ✓ Pass |
| TC04: Agent hiển thị Dashboard | ✓ Pass |
| TC05: Windows Event Log collection | ✓ Pass |
| TC06: Suricata cài đặt | ✓ Pass |
| TC07: Suricata pcap analysis | ✓ Pass |
| TC08: Suricata alerts trong Wazuh | ✓ Pass |
| TC09: Real-time alert < 30s | ✓ Pass |
| TC10: Vulnerability detection | ✓ Pass |

## Security Testing

- TLS encryption Agent-Manager: ✓ Pass
- Dashboard authentication: ✓ Pass
- Indexer isolation: ✓ Pass

## Performance Testing

| Metric | Kết quả | Tiêu chuẩn |
|---|---|---|
| RAM server | ~5.5GB | < 8GB ✓ |
| Alert latency | ~25 giây | < 60s ✓ |
| Startup time | ~2.5 phút | < 3 phút ✓ |
| Agent overhead | ~80MB | < 100MB ✓ |

## Kế hoạch tuần tiếp theo

- Hoàn thiện hệ thống
- Viết User Manual
- Quay video demo

---