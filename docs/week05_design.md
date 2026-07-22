# Thiết kế hệ thống — Tuần 5

## 1. Kiến trúc tổng thể

![Architecture Diagram](architecture.svg)

## 2. Data Flow

[Endpoint — Windows/Linux]
├── Wazuh Agent (native)
│   ├── Thu thập Windows Event Log / syslog
│   ├── File Integrity Monitoring (FIM)
│   ├── Vulnerability scan
│   └── Đọc /var/log/suricata/eve.json
└── Suricata (cài native cùng endpoint)
      └── Lắng nghe network interface
        → Ghi alerts vào eve.json
          ↓ (TLS encrypted, port 1514)
[Wazuh Manager — Docker container]
├── Nhận log từ agents
├── Decode và parse log
├── So khớp rules database
└── Tạo alerts với level 1-15
↓
[Wazuh Indexer — Docker container]
└── Lưu trữ và index events (OpenSearch)
↓
[Wazuh Dashboard — Ubuntu Server]
└── Giao diện web: https://<Wazuh-ip>

## 3. Docker Network

wazuh-network (bridge)
├── wazuh.manager
│   ├── port 1514/tcp  — agent log ingestion
│   ├── port 1515/tcp  — agent enrollment
│   └── port 55000/tcp — REST API
├── wazuh.indexer
│   └── port 9200      — internal only
└── wazuh.dashboard
└── port 443        — exposed to host (HTTPS)

Chỉ Dashboard được expose ra host machine qua port 443.
Wazuh Agent kết nối về Manager qua IP của host machine
port 1514 và 1515.

## 4. Wazuh Alert Schema

### 4.1 Security Event (từ Wazuh Agent)
| Trường | Kiểu | Mô tả |
|---|---|---|
| timestamp | datetime | Thời gian sự kiện xảy ra |
| agent.id | string | ID agent (001, 002...) |
| agent.name | string | Hostname của endpoint |
| agent.ip | string | IP của endpoint |
| rule.id | integer | ID rule kích hoạt alert |
| rule.description | string | Mô tả loại tấn công |
| rule.level | integer | Severity (1-15) |
| rule.groups | array | Nhóm rule (authentication, scan...) |
| data.srcip | string | IP nguồn gây ra sự kiện |
| data.dstip | string | IP đích bị ảnh hưởng |
| full_log | string | Nội dung log gốc từ endpoint |

### 4.2 Suricata EVE Alert (từ Network IDS)
| Trường | Kiểu | Mô tả |
|---|---|---|
| timestamp | datetime | Thời gian alert |
| event_type | string | alert / dns / http / tls / flow |
| src_ip | string | IP nguồn tấn công |
| src_port | integer | Port nguồn |
| dest_ip | string | IP đích |
| dest_port | integer | Port đích |
| proto | string | TCP / UDP / ICMP |
| alert.signature | string | Tên rule (ET SCAN Nmap...) |
| alert.severity | integer | 1=critical, 3=low |
| alert.category | string | Attempted Recon, Malware... |

## 5. Cấu hình Suricata

### suricata.yaml (phần EVE JSON output)
```yaml
outputs:
  - eve-log:
      enabled: yes
      filetype: regular
      filename: /var/log/suricata/eve.json
      types:
        - alert
        - dns
        - http
        - tls
        - flow

default-rule-path: /var/lib/suricata/rules
rule-files:
  - suricata.rules
```

## 6. Cấu hình Wazuh Agent đọc Suricata

### ossec.conf — Suricata integration
```xml
<ossec_config>
  <localfile>
    <log_format>json</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>

  <syscheck>
    <frequency>300</frequency>
    <directories>/etc,/usr/bin,/usr/sbin</directories>
  </syscheck>
</ossec_config>
```

## 7. Wazuh Dashboard Layout

### Security Overview

┌────────────────────────────────────────────────┐
│    Wazuh — Security Dashboard                  │
├──────────┬──────────┬───────────┬──────────────┤
│  Alerts  │  Agents  │ Level 12+ │   Suricata   │
│  1,247   │ 3 Active │     15    │  87 alerts   │
├──────────┴──────────┴───────────┴──────────────┤
│  [Security events timeline — last 24h]         │
├──────────────────────┬─────────────────────────┤
│  [Top 5 rules]       │  [Agents status]        │
│  Authentication fail │  001 DESKTOP ● Active   │
│  ET SCAN Nmap        │  002 LAPTOP  ● Active   │
│  Suricata Alert      │  003 SERVER  ○ Down     │
└──────────────────────┴─────────────────────────┘

## 8. Tiêu chuẩn đánh giá sản phẩm

| Tiêu chí | Mức đạt | Mức tốt |
|---|---|---|
| Server deployment | Ubuntu Server hoạt động | Khởi động < 2 phút / Cài đặt < 10 phút | 
| Agent connect | Agent kết nối Manager | < 30 giây |
| Real-time alert | < 60 giây | < 30 giây |
| Suricata | Phát hiện port scan | Full ET ruleset active |
| Dashboard | Hiển thị events + agents | Auto-refresh real-time |
| RAM server | < 8GB | < 6GB |
| Tài liệu | README đầy đủ | Video demo 5 phút |