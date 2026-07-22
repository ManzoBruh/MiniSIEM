# Nhật ký tuần 10 — Báo cáo thực tập

**Ngày:** Tháng 8, 2026
**Địa điểm:** Làm việc từ xa
**Người hướng dẫn:** Châu Văn Mau

## Công việc đã thực hiện

- Đánh giá mức độ hoàn thành so với tiêu chuẩn tuần 4
- Chuẩn bị slide demo giữa kỳ
- Demo hệ thống cho mentor
- Nhận phản hồi và lập danh sách cải tiến

## Kết quả đánh giá

| Tiêu chí | Tiêu chuẩn | Kết quả |
|---|---|---|
| Server deploy | < 3 phút | ~2.5 phút ✓ |
| Agent connect | < 30 giây | ~20 giây ✓ |
| Real-time alert | < 60 giây | ~25 giây ✓ |
| Suricata | Port scan | ET rules ✓ |
| RAM | < 8GB | ~5.5GB ✓ |

## Phản hồi từ mentor

Mentor rất ấn tượng với tính năng real-time alerting:
"Nó thực sự hoạt động như SIEM doanh nghiệp."
Góp ý cải thiện:
- Cần README rõ ràng hơn cho người mới
- Nên có script tự động hóa cài Agent
- Dashboard cần thêm filter theo agent

## Kế hoạch tuần tiếp theo

- Viết README chi tiết
- Tối ưu Suricata rules
- Thêm custom Wazuh detection rules

---

## Minh chứng

| Minh chứng | File |
|---|---|
| Demo screenshots | evidence/week10/demo.png |
| Mid-term slides | evidence/week10/slides.pdf |
| Mentor feedback | evidence/week10/mentor_feedback.png |