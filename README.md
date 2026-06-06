# MiniSIEM - Personal Security Dashboard

Đây là một hệ thông Security Information and Event Management (SIEM) Dashboard nhỏ nhằm mục đích cho cá nhân và một hệ thống công ty quy mô nhỏ
MiniSIEM cho phép người dùng:
- Upload packet capture từ Wireshark
- Theo dõi trực tiếp hệ thống mạng
- Nhận thông báo thời gian thực về các hành vi lạ
- Tất cả từ một browser-based dashboard

----

## Tính năng

- **PCAP file analyst** - Upload '.pcap' và in ra tất cả nội dung đang chạy trên network
- **Threat classification** - Tự động Flag event dưới dạng Critical, Severe, Caution, Low, Notice, Suspicious
- **AbuseIPDB intergration** - So chéo IP dựa trên hệ thống database kiểm IP
- **ARP spoof detection** - Cảnh báo khi có nguy cơ bị Man-in-the-middle trong hệ thống mạng
- **Live traffic monitoring** - Thu thập và phân tích hệ thống mạng theo thời gian thực
- **SQLite event storage** - Tất cả event được lưu trữ trên lịch sử, tìm kiếm và dữ liệu truy vấn
- **Export report** - Cho phép xuất thông tin dưới dạng CSV

----

## Tech Stack

| Layer | Technology |
|---|---|
| Dashboard UI | Python + Streamlit |
| Packet parsing | Scapy |
| Database | SQLite |
| Threat intel | AbuseIPDB API, VirusTotal API |
| Version control | Git + GitHub |

---

## Cấu trúc dự án

MiniSIEM/
├── app/
│   ├── main.py              # Streamlit dashboard
│   ├── parser.py            # Pcap file parser
│   ├── detection.py         # Detection rule engine
│   ├── database.py          # SQLite handler
│   ├── threat_intel.py      # AbuseIPDB + VirusTotal API calls
│   └── arp_monitor.py       # Live ARP spoof detection
├── data/
│   └── events.db            # Local SQLite database
├── tests/
│   └── test_parser.py       # Unit tests
├── docs/
│   └── architecture.png     # System architecture diagram
├── requirements.txt
└── README.md

--- 

## Bắt đầu

### Yêu cầu
- Python 3.10+
- Tshark / Wireshark
- AbuseIPDB API Key

### Cài đặt

```bash
git clone https://github.com/ManzoBruh/MiniSIEM.git
cd MiniSIEM
pip install -r requirements.txt
streamlit run app/main.py
```

---

## Cấp độ báo động

| Level | Màu sắc | Ý nghĩa |
|---|---|---|
| Critical | 🔴 Red | Xác nhận đang bị tấn công, Malware C2, exploit |
| Severe | 🟠 Orange | Mức đe dọa có tín hiệu cao, port scanning, brute force |
| Suspicious | 🟡 Yellow | Hành vi bất thường cao, ports lạ, traffic có hướng di chuyển lạ |
| Caution | 🔵 Blue | Không nhận ra IP, truy cập từ nguồn ngoài, truy cập đột biến |
| Low | 🟢 Green | Hành vi bất thường thấp, không đáng kể nhưng nên log lại |
| Notice | ⚪ Grey | Chỉ dạng thông tin, services phổ thông, whitelisted IPs |

---


## Roadmap

- [x] Project setup & architecture design
- [ ] Pcap parser
- [ ] AbuseIPDB integration
- [ ] Detection Level (6-level phân cấp: Critical → Notice)
- [ ] Streamlit dashboard
- [ ] Live traffic capture
- [ ] ARP spoof detection
- [ ] VirusTotal integration
- [ ] Đầy đủ documentation & hướng dẫn sử dụng

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

> Được xây dựng thuộc dự án thực tập phù hợp với yêu cầu tiếp cận
> thực tiễn trong công cụ an ninh mạng.
