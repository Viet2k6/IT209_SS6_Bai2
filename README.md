# Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo

## 1. Mục tiêu

Tạo group `devops-admin`, user `deployer` và cấp quyền giới hạn cho group này được sử dụng `systemctl` với các lệnh `start`, `stop`, `restart`, `status` mà không cần nhập mật khẩu.

## 2. Tạo group và user

Tạo group:

```bash
sudo groupadd devops-admin
```

Tạo user:

```bash
sudo adduser deployer
```

Thêm user vào group:

```bash
sudo usermod -aG devops-admin deployer
```

Kiểm tra:

```text
deployer : deployer users devops-admin
```

## 3. Cấu hình sudoers

Mở file sudoers bằng:

```bash
sudo visudo
```

Thêm dòng:

```text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

## 4. Kiểm tra quyền sudo

Chuyển sang user `deployer`:

```bash
su - deployer
```

Kiểm tra quyền:

```bash
sudo -l
```

Kết quả:

```text
User deployer may run the following commands on DESKTOP-1FS54UT:
    (ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

## 5. Kiểm tra restart dịch vụ

Thực hiện:

```bash
sudo systemctl restart cron
```

Lệnh thực hiện thành công và không yêu cầu nhập mật khẩu.

## 6. Kết luận

Đã hoàn thành việc phân quyền cho group `devops-admin`. User `deployer` chỉ được phép sử dụng các lệnh `systemctl start`, `stop`, `restart`, `status` với quyền sudo và không cần nhập mật khẩu.
