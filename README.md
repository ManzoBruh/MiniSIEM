# 🛡️ MiniSIEM — Lightweight Distributed Security Monitoring System

**English** | [Tiếng Việt](#-minisiem--hệ-thống-giám-sát-an-toàn-thông-tin-tinh-gọn-1)

MiniSIEM is an open-source distributed **SIEM (Security Information and Event Management)** system optimized for personal environments, lab setups, and small-to-medium enterprises. It combines the powerful centralized log analysis of **Wazuh** with real-time network intrusion detection from **Suricata IDS**, providing comprehensive threat detection and response capabilities.

**Key Features:**
- ✅ Real-time network traffic analysis with Suricata IDS
- ✅ Centralized log collection and correlation via Wazuh
- ✅ Advanced threat detection using Emerging Threats rulesets
- ✅ Interactive dashboards and threat visualization
- ✅ Low-resource endpoint monitoring
- ✅ Easy-to-deploy architecture on VirtualBox or cloud environments

---

## ⚡ Quick Start (5 Minutes)

### Prerequisites
- **Ubuntu Server 24.04** (4GB RAM, 20GB storage minimum)
- **Windows Endpoint** (Windows 10/11)
- **Oracle VirtualBox** with bridged networking
- Administrator access on both machines

### Installation

**1. Deploy Wazuh Manager (Ubuntu Server)**
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```
⚠️ Save the credentials printed at the end.

**2. Install Windows Endpoint Agent**
```powershell
# Create log directory (run as Admin)
New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"

# Download Suricata rules
Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules" `
  -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"
```

**3. Configure Wazuh Agent**
Add to `C:\Program Files (x86)\ossec-agent\ossec.conf`:
```xml
<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>
```

Restart the agent:
```powershell
Restart-Service -Name WazuhSvc
```

---

## 🏗️ System Architecture

```
[ Network Traffic / Attacker ]
             ↓
[ Windows Endpoint (Victim Machine) ]
  ├── Suricata IDS (Real-time packet analysis)
  │     └─ Alert logs → C:\...\suricata\log\eve.json
  └── Wazuh Agent (File monitoring & secure log forwarding)
             ↓  (Port 1514 - Bridged Network)
[ Ubuntu Server 24.04 (Wazuh Manager) ]
  ├── Wazuh Indexer (Storage & indexing)
  ├── Wazuh Server (Severity scoring & rule matching)
  └── Wazuh Dashboard (Visual analytics interface)
             ↓
[ Administrator (Web Browser - HTTPS) ]
```

---

## 🧰 Tech Stack

| Component | Technology | Description |
|-----------|-----------|-------------|
| **Analysis Server** | Ubuntu Server 24.04 | Open-source Linux optimized for server workloads |
| **SIEM & XDR Core** | Wazuh Manager 4.x | Centralized log collection, analysis, OpenSearch Dashboards |
| **Network IDS** | Suricata 7.0 (Windows) | Deep packet inspection (DPI) with Emerging Threats ruleset |
| **Log Collector** | Wazuh Agent | Lightweight background service, auto-pushes eve.json to manager |
| **Environment** | Oracle VirtualBox | Bridged networking for physical-like connectivity |

---

## 📋 Detailed Installation Guide

### Step 1: Install Wazuh Manager (Ubuntu Server 24.04)

```bash
# Download and run Wazuh installation script
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

**Save your credentials:**
```
Username: admin
Password: [generated-password]
URL: https://[server-ip]:443
```

Access the dashboard at `https://[your-server-ip]` in your browser.

---

### Step 2: Configure Windows Endpoint

#### 2.1 Create Log Directory
```powershell
# Open PowerShell as Administrator
New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"
```

#### 2.2 Update Suricata Configuration
Edit `C:\Users\Admin\MiniSIEM\suricata\suricata.yaml`:
```yaml
default-log-dir: C:/Users/Admin/MiniSIEM/suricata/log
```
*(Note: Use forward slashes `/` in Windows paths for YAML)*

#### 2.3 Download Threat Rulesets
```powershell
Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules" `
  -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"
```

#### 2.4 Configure Wazuh Agent
Edit `C:\Program Files (x86)\ossec-agent\ossec.conf` and add:
```xml
<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>
```

Restart the agent:
```powershell
Restart-Service -Name WazuhSvc
```

---

### Step 3: Start Suricata on Windows

```powershell
cd "C:\Users\Admin\MiniSIEM\suricata"
.\suricata.exe -c suricata.yaml -i "\Device\NPF_{YOUR_ADAPTER_ID}" -S suricata.rules -vv
```

To find your adapter ID:
```powershell
.\suricata.exe --list-interfaces
```

---

## 🚀 Testing & Demo Scenarios

### Scenario 1: Simulate Botnet Traffic
```powershell
# Open new PowerShell and force IPv4
curl.exe -4 -A "ZmEu" http://testmynids.org?botnet=1
```

**Expected Result:** Dashboard immediately alerts with **Critical/High severity** flags.

---


### Scenario 2: Web Application Attack Simulation
```bash
# From Linux machine
sudo apt install sqlmap -y
sqlmap -u "http://target/login.php" --forms --batch
```

**Expected Result:** Wazuh detects SQL injection patterns; dashboard highlights as Web Attack.

---

## 📊 Custom Dashboards

MiniSIEM provides specialized visualizations on OpenSearch Dashboards to transform Wazuh into a full **SOC (Security Operations Center)**:

- **🎯 The Threat Landscape** (Pie Chart)
  - Alert severity distribution (Critical, High, Medium, Low)
  - Identify top threat categories at a glance

- **📈 Top Attack Vectors** (Bar Chart)
  - Most frequently detected attack types
  - Based on Suricata signature matches
  
- **🔴 Attacker IPs** (Data Table)
  - Ranked list of malicious IP addresses
  - Direct export for blocking/reporting

- **⏱️ Real-Time Alert Feed** (Timeline)
  - Live-updating event stream
  - Sort by severity, timestamp, or attack type

---

## 📁 Project Structure

```
MiniSIEM/
├── docs/
│   └── User_Guide_VN.md              # Vietnamese user guide & troubleshooting
├── suricata/
│   ├── suricata.yaml                 # Suricata engine configuration
│   └── suricata.rules                # (Ignored) Dynamically downloaded ruleset
├── wazuh/
│   └── ossec.conf                    # Sample Wazuh agent configuration
├── .gitignore                        # Excludes .exe and local logs
└── README.md                         # This file
```

---

## 🎯 Core Features Implemented

- ✅ Successfully integrated Suricata IDS into Wazuh Manager architecture
- ✅ Real-time JSON (eve.json) stream collection and parsing
- ✅ Powerful threat detection: Scanning, Botnet, Web Vulnerabilities
- ✅ Flexible data filtering on OpenSearch with centralized dashboard management
- ✅ Optimized memory footprint for endpoints (low CPU/RAM consumption)
- ✅ Automated alert correlation and severity scoring

---

## 🛠️ Troubleshooting

### Agent Not Connecting to Manager
```bash
# Check agent status on Windows
"C:\Program Files (x86)\ossec-agent\agent-control.exe" -l

# Verify manager IP in ossec.conf
cat "C:\Program Files (x86)\ossec-agent\ossec.conf" | grep -A 5 "<server>"

# Restart Wazuh service
Restart-Service -Name WazuhSvc
```

### No Logs Appearing in Dashboard
- Confirm Suricata is running: `Get-Process suricata`
- Check eve.json file exists: `Test-Path "C:\Users\Admin\MiniSIEM\suricata\log\eve.json"`
- Verify JSON format: `Get-Content "C:\Users\Admin\MiniSIEM\suricata\log\eve.json" | Select -First 5`

### Network Connectivity Issues
- Verify bridged network: `ipconfig /all` should show 192.168.x.x range
- Test connectivity: `Test-NetConnection -ComputerName [UBUNTU_IP] -Port 1514`
- Check Windows Firewall: Allow port 1514 outbound

### Still Having Issues?
Refer to `docs/User_Guide_VN.md` for detailed solutions or official [Wazuh Documentation](https://documentation.wazuh.com/).

---

## 📚 Resources & Documentation

- [Wazuh Official Documentation](https://documentation.wazuh.com/)
- [Suricata User Guide](https://suricata.io/documentation/)
- [Emerging Threats Ruleset](https://rules.emergingthreats.net/)
- [OpenSearch Dashboards](https://opensearch.org/docs/latest/dashboards/index/)
- [Wazuh Agent for Windows](https://packages.wazuh.com/4.x/windows/)

---

## 📝 Project Information

- **Internship Program:** DuLi Computer Shop
- **Mentor:** Châu Văn Mau
- **Developer:** ManzoBruh
- **Status:** Active & Maintained ✨

---

---

# 🛡️ MiniSIEM — Hệ Thống Giám Sát An Toàn Thông Tin Tinh Gọn

**[English](#-minisiem--lightweight-distributed-security-monitoring-system)** | **Tiếng Việt**

MiniSIEM là một hệ thống **SIEM (Security Information and Event Management)** phân tán mã nguồn mở, được tối ưu hóa cho các môi trường cá nhân, phòng lab hoặc doanh nghiệp quy mô nhỏ. 

Hệ thống kết hợp sức mạnh phân tích log tập trung của **Wazuh** và khả năng phát hiện xâm nhập mạng (IDS) theo thời gian thực của **Suricata Engine**, giúp phát hiện và ứng phó với các mối đe dọa bảo mật một cách toàn diện.

**Tính Năng Chính:**
- ✅ Phân tích lưu lượng mạng real-time với Suricata IDS
- ✅ Thu thập và tương quan log tập trung qua Wazuh
- ✅ Phát hiện mối đe dọa nâng cao sử dụng tập luật Emerging Threats
- ✅ Bảng điều khiển tương tác và trực quan hóa mối đe dọa
- ✅ Giám sát Endpoint tiêu thụ tài nguyên thấp
- ✅ Kiến trúc dễ triển khai trên VirtualBox hoặc môi trường cloud

---

## ⚡ Bắt Đầu Nhanh (5 Phút)

### Yêu Cầu
- **Ubuntu Server 24.04** (tối thiểu 4GB RAM, 20GB lưu trữ)
- **Windows Endpoint** (Windows 10/11)
- **Oracle VirtualBox** có cấu hình mạng Bridged
- Quyền Administrator trên cả hai máy

### Cài Đặt

**1. Triển Khai Wazuh Manager (Ubuntu Server)**
```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```
⚠️ Ghi lại thông tin đăng nhập được hiển thị cuối cùng.

**2. Cài Đặt Agent Trên Windows**
```powershell
# Tạo thư mục log (chạy quyền Admin)
New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"

# Tải tập luật Suricata
Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules" `
  -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"
```

**3. Cấu Hình Wazuh Agent**
Thêm vào `C:\Program Files (x86)\ossec-agent\ossec.conf`:
```xml
<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>
```

Khởi động lại agent:
```powershell
Restart-Service -Name WazuhSvc
```

---

## 🏗️ Kiến Trúc Hệ Thống

```
[ Lưu Lượng Mạng / Kẻ Tấn Công ]
             ↓
[ Windows Endpoint (Máy Nạn Nhân) ]
  ├── Suricata IDS (Phân tích gói tin real-time)
  │     └─ Ghi log cảnh báo → C:\...\suricata\log\eve.json
  └── Wazuh Agent (Theo dõi file & Chuyển tiếp log an toàn)
             ↓  (Gửi log qua Port 1514 - Bridged Network)
[ Ubuntu Server 24.04 (Wazuh Manager) ]
  ├── Wazuh Indexer (Lưu trữ và đánh chỉ mục)
  ├── Wazuh Server (Chấm điểm Severity & So khớp tập luật)
  └── Wazuh Dashboard (Giao diện hiển thị trực quan)
             ↓
[ Quản Trị Viên (Web Browser - HTTPS) ]
```

---

## 🧰 Tech Stack

| Thành Phần | Công Nghệ | Mô Tả |
|-----------|-----------|-------|
| **Máy Chủ Phân Tích** | Ubuntu Server 24.04 | Nền tảng Linux mã nguồn mở, tối ưu tài nguyên cho Server |
| **SIEM & XDR Core** | Wazuh Manager 4.x | Thu thập log tập trung, phân tích, OpenSearch Dashboard |
| **Network IDS** | Suricata 7.0 (Windows) | Kiểm tra sâu gói tin (DPI), tập luật Emerging Threats |
| **Log Collector** | Wazuh Agent | Dịch vụ siêu nhẹ, tự động đẩy eve.json về trung tâm |
| **Môi Trường** | Oracle VirtualBox | Mạng Bridged mô phỏng kết nối phần cứng vật lý |

---

## 📋 Hướng Dẫn Cài Đặt Chi Tiết

### Bước 1: Cài Đặt Wazuh Manager (Ubuntu Server 24.04)

```bash
# Tải script cài đặt Wazuh
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

**Ghi lại thông tin đăng nhập:**
```
Tên người dùng: admin
Mật khẩu: [mật-khẩu-được-tạo]
URL: https://[ip-server]:443
```

Truy cập dashboard tại `https://[ip-server]` trên trình duyệt.

---

### Bước 2: Cấu Hình Windows Endpoint

#### 2.1 Tạo Thư Mục Log
```powershell
# Mở PowerShell quyền Administrator
New-Item -ItemType Directory -Force -Path "C:\Users\Admin\MiniSIEM\suricata\log"
```

#### 2.2 Cập Nhật Cấu Hình Suricata
Chỉnh sửa `C:\Users\Admin\MiniSIEM\suricata\suricata.yaml`:
```yaml
default-log-dir: C:/Users/Admin/MiniSIEM/suricata/log
```
*(Lưu ý: Dùng dấu `/` trong đường dẫn Windows)*

#### 2.3 Tải Tập Luật Bảo Mật
```powershell
Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-7.0/emerging-all.rules" `
  -OutFile "C:\Users\Admin\MiniSIEM\suricata\suricata.rules"
```

#### 2.4 Cấu Hình Wazuh Agent
Chỉnh sửa `C:\Program Files (x86)\ossec-agent\ossec.conf` và thêm:
```xml
<localfile>
  <log_format>json</log_format>
  <location>C:\Users\Admin\MiniSIEM\suricata\log\eve.json</location>
</localfile>
```

Khởi động lại agent:
```powershell
Restart-Service -Name WazuhSvc
```

---

### Bước 3: Khởi Động Suricata Trên Windows

```powershell
cd "C:\Users\Admin\MiniSIEM\suricata"
.\suricata.exe -c suricata.yaml -i "\Device\NPF_{YOUR_ADAPTER_ID}" -S suricata.rules -vv
```

Để tìm Adapter ID:
```powershell
.\suricata.exe --list-interfaces
```

---

## 🚀 Kịch Bản Kiểm Thử & Demo

### Kịch Bản 1: Giả Lập Botnet
```powershell
# Mở PowerShell mới, ép dùng IPv4
curl.exe -4 -A "ZmEu" http://testmynids.org?botnet=1
```

**Kết Quả:** Dashboard cảnh báo **Critical/High severity** ngay lập tức.

---

### Kịch Bản 2: Giả Lập Tấn Công Ứng Dụng Web
```bash
# Từ máy Linux
sudo apt install sqlmap -y
sqlmap -u "http://target/login.php" --forms --batch
```

**Kết Quả:** Wazuh phát hiện SQL injection; dashboard hiển thị Web Attack.

---

## 📊 Bảng Điều Khiển Tùy Chỉnh

MiniSIEM cung cấp các trực quan hóa chuyên biệt trên OpenSearch Dashboards để biến Wazuh thành một **SOC (Security Operations Center)** thực thụ:

- **🎯 The Threat Landscape** (Biểu Đồ Tròn)
  - Phân phối mức độ cảnh báo (Critical, High, Medium, Low)
  - Nhận biết các loại mối đe dọa hàng đầu một cách nhanh chóng

- **📈 Top Attack Vectors** (Biểu Đồ Cột)
  - Các loại tấn công được phát hiện thường xuyên nhất
  - Dựa trên khớp tập luật Suricata
  
- **🔴 Attacker IPs** (Bảng Dữ Liệu)
  - Danh sách xếp hạng các địa chỉ IP độc hại
  - Xuất trực tiếp để chặn/báo cáo

- **⏱️ Luồng Cảnh Báo Real-Time** (Đồ Thị Thời Gian)
  - Cập nhật sự kiện trực tiếp
  - Sắp xếp theo mức độ nghiêm trọng, dấu thời gian hoặc loại tấn công

---

## 📁 Cấu Trúc Dự Án

```
MiniSIEM/
├── docs/
│   └── User_Guide_VN.md              # Hướng dẫn tiếng Việt & Khắc phục sự cố
├── suricata/
│   ├── suricata.yaml                 # Cấu hình Suricata Engine
│   └── suricata.rules                # (Bị bỏ qua) Tập luật tải động
├── wazuh/
│   └── ossec.conf                    # Cấu hình mẫu Wazuh Agent
├── .gitignore                        # Bỏ qua .exe và log cục bộ
└── README.md                         # File này
```

---

## 🎯 Chức Năng Cốt Lõi Đã Triển Khai

- ✅ Tích hợp thành công Suricata IDS vào kiến trúc Wazuh Manager
- ✅ Thu thập và phân tích luồng JSON (eve.json) real-time
- ✅ Phát hiện mạnh mẽ: Scanning, Botnet, Web Vulnerabilities
- ✅ Lọc dữ liệu linh hoạt trên OpenSearch với quản lý Dashboard tập trung
- ✅ Tối ưu hóa bộ nhớ cho Endpoint (CPU/RAM thấp)
- ✅ Tương quan cảnh báo tự động và chấm điểm mức độ

---

## 🛠️ Khắc Phục Sự Cố

### Agent Không Kết Nối Được Với Manager
```bash
# Kiểm tra trạng thái agent trên Windows
"C:\Program Files (x86)\ossec-agent\agent-control.exe" -l

# Xác minh IP manager trong ossec.conf
cat "C:\Program Files (x86)\ossec-agent\ossec.conf" | grep -A 5 "<server>"

# Khởi động lại dịch vụ Wazuh
Restart-Service -Name WazuhSvc
```

### Không Có Log Nào Xuất Hiện Trên Dashboard
- Xác minh Suricata đang chạy: `Get-Process suricata`
- Kiểm tra eve.json tồn tại: `Test-Path "C:\Users\Admin\MiniSIEM\suricata\log\eve.json"`
- Xác minh định dạng JSON: `Get-Content "C:\Users\Admin\MiniSIEM\suricata\log\eve.json" | Select -First 5`

### Vấn Đề Kết Nối Mạng
- Xác minh mạng Bridged: `ipconfig /all` nên hiển thị dải 192.168.x.x
- Kiểm tra kết nối: `Test-NetConnection -ComputerName [IP_UBUNTU] -Port 1514`
- Kiểm tra Windows Firewall: Cho phép port 1514 outbound

### Vẫn Có Vấn Đề?
Tham khảo `docs/User_Guide_VN.md` để biết chi tiết hoặc [Tài Liệu Wazuh Chính Thức](https://documentation.wazuh.com/).

---

## 📚 Tài Liệu & Tài Nguyên

- [Tài Liệu Wazuh Chính Thức](https://documentation.wazuh.com/)
- [Hướng Dẫn Suricata](https://suricata.io/documentation/)
- [Tập Luật Emerging Threats](https://rules.emergingthreats.net/)
- [OpenSearch Dashboards](https://opensearch.org/docs/latest/dashboards/index/)
- [Wazuh Agent cho Windows](https://packages.wazuh.com/4.x/windows/)

---

## 📝 Thông Tin Dự Án

- **Chương Trình Thực Tập:** Cửa Hàng Vi Tính DuLi
- **Người Hướng Dẫn:** Châu Văn Mau
- **Nhà Phát Triển:** ManzoBruh
- **Trạng Thái:** Hoạt Động & Bảo Trì ✨
