# Nhật ký tuần 6 — Báo cáo thực tập

**Ngày:** Tháng 7, 2026  
**Địa điểm:** Làm việc tại Cửa Hàng Vi Tính DuLi  
**Người hướng dẫn:** Châu Văn Mau, Chủ cửa hàng  

---

## Công việc đã thực hiện trong tuần

Tuần này em thiết lập toàn bộ môi trường phát triển, cài đặt
Docker Desktop, thiết lập Git workflow, và chuẩn bị cấu trúc
dự án trên GitHub.

## Môi trường phát triển

- **OS:** Windows 11
- **Docker Desktop:** v4.x với WSL2 backend
- **Git:** v2.x
- **IDE:** VS Code với extensions Docker, YAML, GitLens
- **Browser:** Chrome (để truy cập Grafana)

## Git workflow

Em thiết lập quy ước commit message theo Conventional Commits:
- `feat:` — tính năng mới
- `fix:` — sửa lỗi
- `docs:` — tài liệu
- `config:` — thay đổi cấu hình
- `test:` — kiểm thử

## Cấu trúc repository

Repository được thiết lập với cấu trúc thư mục đầy đủ theo
thiết kế tuần 5, bao gồm các thư mục cho Zeek, Promtail,
Loki, Grafana config, docs, và tests.

## Những gì đã học được

- Cách cài đặt và cấu hình Docker Desktop trên Windows với WSL2
- Sự khác biệt giữa Docker volume và bind mount
- Cách tổ chức một dự án infrastructure-as-code trên GitHub

## Khó khăn gặp phải

Docker Desktop yêu cầu bật WSL2 và Virtualization trong BIOS.
Quá trình cài đặt mất nhiều thời gian hơn dự kiến do cần
restart máy nhiều lần và cập nhật WSL2 kernel.

## Kế hoạch tuần tiếp theo

- Chuẩn bị slide báo cáo giữa kỳ
- Trình bày thiết kế với mentor
- Xác nhận tech stack và tiêu chuẩn đánh giá

---

## Minh chứng tuần 6

| Minh chứng | File |
|---|---|
| Docker Desktop installed | evidence/week06/docker_desktop.png |
| GitHub repository structure | evidence/week06/github_structure.png |
| VS Code environment | evidence/week06/vscode.png |