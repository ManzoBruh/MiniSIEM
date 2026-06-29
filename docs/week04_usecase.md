# Đặc tả Use Case — Tuần 4

## 1. Use Case Diagram

![Use Case Diagram](usecase_diagram.svg)

## 2. Danh sách Use Case

| ID | Tên Use Case | Actor | Yêu cầu liên quan |
|---|---|---|---|
| UC01 | Xem tổng quan traffic | Người dùng | F04 |
| UC02 | Xem danh sách kết nối | Người dùng | F05 |
| UC03 | Xem DNS queries | Người dùng | F06 |
| UC04 | Xem cảnh báo bảo mật | Người dùng | F07 |
| UC05 | Tìm kiếm log | Người dùng | F08 |
| UC06 | Phân tích file pcap | Người dùng | F09 |
| UC07 | Giám sát mạng real-time | Hệ thống | F01 |
| UC08 | Thu thập và ship log | Hệ thống | F02, F03 |
| UC09 | Tạo cảnh báo tự động | Hệ thống | F07 |
| UC10 | Triển khai hệ thống | Người dùng | F10 |

## 3. Đặc tả chi tiết

### UC01 — Xem tổng quan traffic
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Hệ thống đang chạy, có traffic mạng |
| Luồng chính | 1. Người dùng mở Grafana (localhost:3000) |
| | 2. Hệ thống hiển thị Overview Dashboard |
| | 3. Dashboard tự động cập nhật mỗi 5 giây |
| | 4. Người dùng xem tổng số kết nối, bytes, alerts |
| Kết quả | Người dùng nắm được tình trạng mạng hiện tại |

### UC02 — Xem danh sách kết nối
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Zeek đang chạy và tạo conn.log |
| Luồng chính | 1. Người dùng mở Connection Dashboard |
| | 2. Grafana truy vấn Loki lấy conn.log |
| | 3. Hiển thị bảng: src IP, dst IP, port, protocol, bytes |
| | 4. Người dùng có thể lọc theo IP hoặc port |
| Kết quả | Danh sách kết nối mạng theo thời gian thực |

### UC03 — Xem DNS queries
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Zeek đang tạo dns.log |
| Luồng chính | 1. Người dùng mở DNS Dashboard |
| | 2. Grafana hiển thị DNS queries theo thời gian |
| | 3. Hệ thống đánh dấu queries có volume bất thường |
| Kết quả | Phát hiện DNS tunneling hoặc C2 communication |

### UC04 — Xem cảnh báo bảo mật
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Zeek đang tạo notice.log |
| Luồng chính | 1. Người dùng mở Alert Dashboard |
| | 2. Grafana hiển thị cảnh báo từ notice.log |
| | 3. Mỗi cảnh báo có: thời gian, loại, IP, mô tả |
| Kết quả | Người dùng nhận biết và điều tra các mối đe dọa |

### UC05 — Tìm kiếm log
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Loki đang chứa log data |
| Luồng chính | 1. Người dùng mở Grafana Explore |
| | 2. Nhập LogQL query |
| | 3. Loki trả về kết quả phù hợp |
| | 4. Grafana hiển thị kết quả dạng bảng hoặc log |
| Luồng thay thế | Query không có kết quả → hiển thị thông báo |
| Kết quả | Người dùng tìm được log cần điều tra |

### UC06 — Phân tích file pcap
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Có file pcap cần phân tích |
| Luồng chính | 1. Người dùng chạy Zeek với file pcap |
| | 2. Zeek phân tích và tạo log files |
| | 3. Promtail đẩy log vào Loki |
| | 4. Grafana hiển thị kết quả phân tích |
| Kết quả | Phân tích chi tiết traffic trong file pcap |

### UC07 — Giám sát mạng real-time
| Trường | Nội dung |
|---|---|
| Actor | Hệ thống |
| Điều kiện tiên quyết | Zeek đang chạy trên network interface |
| Luồng chính | 1. Zeek lắng nghe trên interface |
| | 2. Phân tích từng gói tin theo thời gian thực |
| | 3. Ghi log liên tục vào thư mục output |
| Kết quả | Log được cập nhật liên tục theo traffic mạng |

### UC08 — Thu thập và ship log
| Trường | Nội dung |
|---|---|
| Actor | Hệ thống |
| Điều kiện tiên quyết | Promtail đang chạy, Loki đang chạy |
| Luồng chính | 1. Promtail theo dõi thư mục log của Zeek |
| | 2. Phát hiện log mới → gắn label |
| | 3. Đẩy vào Loki qua HTTP API |
| Kết quả | Log được lưu trữ trong Loki để truy vấn |

### UC09 — Tạo cảnh báo tự động
| Trường | Nội dung |
|---|---|
| Actor | Hệ thống |
| Điều kiện tiên quyết | Zeek phát hiện hành vi bất thường |
| Luồng chính | 1. Zeek phát hiện anomaly (port scan, DNS tunnel…) |
| | 2. Ghi vào notice.log |
| | 3. Promtail đẩy vào Loki |
| | 4. Grafana alert rule kích hoạt |
| Kết quả | Cảnh báo hiển thị trên dashboard ngay lập tức |

### UC10 — Triển khai hệ thống
| Trường | Nội dung |
|---|---|
| Actor | Người dùng |
| Điều kiện tiên quyết | Docker Desktop đã được cài đặt |
| Luồng chính | 1. Clone repo từ GitHub |
| | 2. Chạy docker compose up -d |
| | 3. Mở Grafana tại localhost:3000 |
| Kết quả | Hệ thống chạy hoàn chỉnh trong vài phút |

## 4. Tiêu chuẩn đánh giá

| Tiêu chí | Mức đạt | Mức tốt |
|---|---|---|
| Triển khai | Chạy được bằng docker compose up | Chạy được trên Windows/Linux/Mac |
| Zeek | Tạo đủ 6 loại log file | Parse được file pcap thực tế |
| Grafana | Có 3 dashboard cơ bản | Dashboard tự động cập nhật ≤5s |
| Tìm kiếm | LogQL cơ bản hoạt động | Tìm được event theo IP và thời gian |
| Tài nguyên | RAM < 512MB | RAM < 256MB |
| Tài liệu | README đủ hướng dẫn cài đặt | Video demo đầy đủ |