# Bài 2 — Nhóm devops-admin và sudoers (visudo)

## Mục tiêu

Cấp user `deployer` (nhóm `devops-admin`) quyền chạy `systemctl` start/stop/restart/status **không mật khẩu**, không full root.

## Các lệnh đã chạy

```bash
sudo groupadd devops-admin
sudo adduser deployer
sudo usermod -aG devops-admin deployer

sudo visudo
```

## Dòng cấu hình thêm vào `/etc/sudoers`

```text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

- `%devops-admin` = mọi user trong nhóm
- `NOPASSWD` = không hỏi mật khẩu
- Chỉ các lệnh `systemctl` nêu trên, không phải toàn bộ root

## Kiểm tra

```bash
su - deployer
sudo -l
sudo systemctl restart cron
```

### Kết quả `sudo -l` (mẫu)

```text
User deployer may run the following commands on <hostname>:
    (ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *,
                    /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

`sudo systemctl restart cron` chạy thành công, không hỏi password.

## Ghi chú

Luôn sửa sudoers bằng `visudo` (kiểm tra cú pháp, tránh khóa mất quyền sudo).
