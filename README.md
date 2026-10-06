# Báo cáo Bài 1: Khảo sát FHS và Phân quyền File/Folder Nâng cao

## 1. Thông tin chung
- **Bài tập:** Bài 1 - Khảo sát FHS và Phân quyền File/Folder nâng cao
- **Đường dẫn nộp bài:** `homework/session_06/ex1/README.md`
- **Mục tiêu:** Thực hành khởi tạo cấu trúc thư mục chuẩn FHS tại `/var/www/my-app`, thiết lập phân quyền (`chmod`) và gán quyền sở hữu (`chown`) cho ứng dụng Web.

---

## 2. Nhật ký thực thi các lệnh (Command Logs)

### Bước 1: Khởi tạo cấu trúc thư mục
Sử dụng đặc quyền `sudo` để tạo cây thư mục cho ứng dụng web công khai và thư mục chứa nhật ký hệ thống:

```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

### Bước 2: Thay đổi quyền sở hữu và nhóm sở hữu
Thay đổi chủ sở hữu thư mục về tài khoản người dùng hiện tại (`$USER`) và nhóm sở hữu về `www-data` cho toàn bộ cấu trúc thư mục `/var/www/my-app`:

```bash
sudo chown -R $USER:www-data /var/www/my-app
```

### Bước 3: Phân quyền cho các thư mục
- **Thư mục `public`:** Gán quyền `750` (`drwxr-x---`)
  - **Owner:** Toàn quyền (Đọc, Ghi, Điều hướng - `rwx`)
  - **Group:** Chỉ đọc và điều hướng (`r-x`)
  - **Others:** Không có quyền (`---`)

```bash
sudo chmod 750 /var/www/my-app/public
```

- **Thư mục `logs`:** Gán quyền `770` (`drwxrwx---`)
  - **Owner:** Toàn quyền (Đọc, Ghi, Điều hướng - `rwx`)
  - **Group:** Toàn quyền (Đọc, Ghi, Điều hướng - `rwx`)
  - **Others:** Không có quyền (`---`)

```bash
sudo chmod 770 /var/www/my-app/logs
```

---

## 3. Kiểm tra kết quả (Verification)

Thực hiện kiểm tra lại cấu trúc thư mục và phân quyền bằng lệnh `ls -la /var/www/my-app`:

```bash
ls -la /var/www/my-app
```

**Đầu ra của lệnh (Output):**

```text
total 16
drwxr-xr-x 4 student www-data 4096 Oct  6 15:30 .
drwxr-xr-x 3 root    root     4096 Oct  6 15:30 ..
drwxrwx--- 2 student www-data 4096 Oct  6 15:30 logs
drwxr-x--- 2 student www-data 4096 Oct  6 15:30 public
```

---

## 4. Đánh giá và Đối chiếu yêu cầu

| Thư mục | Quyền ký tự | Quyền số | Owner | Group | Khớp yêu cầu |
| :--- | :---: | :---: | :---: | :---: | :---: |
| `/var/www/my-app/public` | `drwxr-x---` | `750` | `$USER` | `www-data` | Đạt |
| `/var/www/my-app/logs` | `drwxrwx---` | `770` | `$USER` | `www-data` | Đạt |