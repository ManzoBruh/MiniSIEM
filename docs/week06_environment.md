# Thiết lập môi trường — Tuần 6

## 1. Yêu cầu hệ thống

| Thành phần | Yêu cầu tối thiểu | Khuyến nghị |
|---|---|---|
| OS | Windows 10/11, Linux, macOS | Windows 11 hoặc Ubuntu 22.04 |
| RAM | 4GB | 8GB+ |
| CPU | 2 cores | 4 cores+ |
| Storage | 10GB free | 20GB+ |
| Docker | Docker Desktop 4.0+ | Mới nhất |

## 2. Cài đặt Docker Desktop (Windows)

```bash
# 1. Tải Docker Desktop từ docker.com
# 2. Bật WSL2:
wsl --install
wsl --set-default-version 2

# 3. Cài đặt Docker Desktop
# 4. Kiểm tra:
docker --version
docker compose version
```

## 3. Cấu trúc thư mục dự án

MiniSIEM/
├── docker-compose.yml
├── .env.example
├── zeek/config/
├── promtail/config/
├── loki/config/
├── grafana/
│   ├── config/
│   ├── dashboards/
│   └── provisioning/
├── docs/
├── samples/
└── tests/

## 4. Git workflow

```bash
# Clone repo
git clone https://github.com/ManzoBruh/MiniSIEM.git
cd MiniSIEM

# Tạo .env từ template
cp .env.example .env

# Commit convention
git commit -m "feat: add zeek configuration"
git commit -m "config: update loki retention policy"
git commit -m "docs: add week 6 diary"
```

## 5. VS Code extensions

- Docker (ms-azuretools.vscode-docker)
- YAML (redhat.vscode-yaml)
- GitLens (eamodio.gitlens)
- Remote - WSL (ms-vscode-remote.remote-wsl)