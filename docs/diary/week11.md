# Nhật ký tuần 11 — Báo cáo thực tập

**Ngày:** Tháng 8, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

### Sửa lỗi
- BUG-01: Alert latency cao do Agent buffer — sửa bằng
  cách giảm agent.conf queue size
- BUG-02: Suricata EVE JSON không parse đúng timezone —
  fix bằng cách set UTC trong suricata.yaml
- BUG-03: Wazuh Dashboard timeout sau 1 giờ — fix session
  timeout trong opensearch config

### Cải tiến
- Thêm custom Wazuh rule phát hiện nmap scan
- Cập nhật Emerging Threats rules lên phiên bản mới nhất
- Tối ưu Wazuh Indexer memory settings: giảm từ 6GB → 4GB heap
- Viết README chi tiết với step-by-step guide

## Danh sách bug đã sửa

| Bug | Mô tả | Trạng thái |
|---|---|---|
| BUG-01 | Alert latency cao | ✅ Đã sửa |
| BUG-02 | EVE JSON timezone | ✅ Đã sửa |
| BUG-03 | Dashboard timeout | ✅ Đã sửa |

## Kế hoạch tuần tiếp theo

- Functional Testing đầy đủ
- Security Testing
- Performance Testing

---

## Minh chứng

| Minh chứng | File |
|---|---|
| Bug list | evidence/week11/bug_list.png |
| Custom rule | evidence/week11/custom_rule.png |
| Optimized RAM | evidence/week11/ram_usage.png |