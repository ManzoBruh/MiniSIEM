# Nhật ký tuần 1 — Báo cáo thực tập

**Ngày:** Tháng 6, 2026  
**Địa điểm:** Làm việc tại Cửa Hàng Vi Tính DuLi  
**Người hướng dẫn:** Châu Văn Mau, Chủ cửa hàng  

---

## Công việc đã thực hiện trong tuần

Trong tuần đầu tiên, em tập trung vào các công việc hành chính và
khởi động dự án. Cụ thể:

- Liên hệ và xác nhận vị trí thực tập tại Cửa Hàng Vi Tính DuLi
- Hoàn thiện và nộp Đơn đăng ký thực tập
- Trao đổi trực tiếp với anh Châu Văn Mau về định hướng dự án
  và kỳ vọng trong suốt 16 tuần thực tập
- Tiếp nhận chính thức vị trí thực tập sinh phát triển hệ thống
  bảo mật tại cơ sở
- Tạo repository GitHub công khai cho dự án MiniSIEM
- Thiết lập cấu trúc thư mục dự án theo tiêu chuẩn
- Soạn thảo kế hoạch thực tập 16 tuần

## Nội dung trao đổi với mentor

Trong buổi gặp đầu tiên với anh Châu Văn Mau, em đã trình bày
ý tưởng về dự án MiniSIEM — một hệ thống giám sát bảo mật mạng
cá nhân sử dụng các công nghệ mã nguồn mở. Xuất phát từ kinh
nghiệm làm việc thực tế trong lĩnh vực bảo mật thông tin, em
nhận thấy rằng môi trường IT cá nhân và hộ gia đình thường bị
bỏ ngỏ về mặt bảo mật — không có công cụ giám sát, không có
cảnh báo sớm khi có xâm nhập hay tấn công mạng.

Trong khi đó, các giải pháp enterprise như Splunk hay IBM QRadar
có chi phí rất cao, nằm ngoài tầm với của người dùng cá nhân.
MiniSIEM ra đời nhằm lấp đầy khoảng trống này — mang lại một
hệ thống giám sát bảo mật miễn phí, nhẹ, dễ triển khai, sử dụng
các công nghệ mã nguồn mở đã được kiểm chứng trong môi trường
doanh nghiệp.

Anh Mau đồng ý với hướng đi này và xác nhận sẽ hỗ trợ
em trong suốt quá trình thực tập.

## Tech stack được đề xuất

Sau buổi trao đổi, em đề xuất sử dụng các công nghệ sau:

| Tầng | Công nghệ | Lý do |
|---|---|---|
| Network IDS | Zeek | Phân rã dữ liệu mạng chi tiết dạng JSON, rất nhẹ |
| Log Shipper | Promtail | Thu thập và chuyển tiếp log thời gian thực |
| Log Database | Grafana Loki | Lưu trữ log tối ưu, không cần index toàn bộ dữ liệu |
| Visualization | Grafana | Dashboard giám sát real-time, truy vấn bằng LogQL |
| Triển khai | Docker Compose | Cô lập môi trường, chạy hệ thống bằng 1 câu lệnh |
| Quản lý mã nguồn | Git + GitHub | Chuẩn công nghiệp |

## Kết quả đạt được

- ✅ Hoàn thiện đơn đăng ký thực tập
- ✅ Nhận xác nhận tiếp nhận từ cơ sở thực tập
- ✅ Tạo GitHub repository: github.com/ManzoBruh/MiniSIEM
- ✅ Thiết lập cấu trúc thư mục dự án
- ✅ Soạn thảo kế hoạch thực tập 16 tuần

## Những gì đã học được

- Hiểu rõ hơn về khoảng cách giữa giải pháp bảo mật enterprise
  và nhu cầu thực tế của người dùng cá nhân
- Nắm được tổng quan về các công nghệ mã nguồn mở trong lĩnh
  vực giám sát mạng: Zeek, Loki, Grafana, Promtail
- Học cách trình bày ý tưởng dự án kỹ thuật một cách rõ ràng
  và thuyết phục với người không chuyên về lập trình

## Khó khăn gặp phải

Việc xác định đúng phạm vi dự án trong tuần đầu là thách thức
lớn nhất. Ban đầu em muốn tích hợp quá nhiều tính năng cùng lúc,
nhưng sau khi trao đổi với mentor, đã quyết định tập trung vào
việc xây dựng một hệ thống hoàn chỉnh và ổn định trước khi mở
rộng thêm tính năng.

## Kế hoạch tuần tiếp theo

- Nghiên cứu chi tiết về kiến trúc và cách hoạt động của Zeek,
  Promtail, Loki và Grafana
- Đọc tài liệu chính thức của từng công nghệ
- Thu thập tài liệu kỹ thuật liên quan
- Ghi chép kết quả nghiên cứu vào tài liệu week02_research.md

---

## Minh chứng tuần 1

| Minh chứng | File |
|---|---|
| Đơn đăng ký thực tập | evidence\Hợp đồng thực tập.pdf |
| Cấu trúc thư mục dự án | evidence/week01/folder_structure.png |