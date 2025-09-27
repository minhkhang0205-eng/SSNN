# Quản lý thanh niên sẵn sàng nhập ngũ (React + Electron)


## Mục tiêu của gói này
Gói mã nguồn này đã **kèm sẵn GitHub Actions** (file `.github/workflows/build.yml`) để khi bạn upload toàn bộ thư mục lên **GitHub** và push lên nhánh `main`, GitHub sẽ tự động chạy workflow để **build file cài Windows (.exe)** và lưu kết quả ở phần **Artifacts** (bạn có thể tải về trực tiếp từ trang Actions).

---

## Hướng dẫn chi tiết (bước—bước)

### A. Tạo repository trên GitHub (một lần)
1. Đăng nhập GitHub (https://github.com). Nếu chưa có tài khoản, đăng ký.
2. Tạo repository mới: nhấp **+ → New repository**.
3. Đặt tên repository, ví dụ `quan-ly-nhap-ngu`. Chọn **Public** (để dễ tải artifact). Bấm **Create repository**.

### B. Upload mã nguồn lên GitHub (có 2 cách)

#### Cách 1 — Dùng Git (khuyến nghị, ổn định với nhiều file)
1. Cài Git: https://git-scm.com/download/win
2. Mở Git Bash hoặc CMD, chuyển đến thư mục chứa project đã giải nén, ví dụ:
   ```bash
   cd C:\path\to\quan-ly-nhap-ngu-with-workflow
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/quan-ly-nhap-ngu.git
   git push -u origin main
   ```
3. Thay `YOUR_USERNAME` bằng tên tài khoản GitHub của bạn. Lần đầu push có thể yêu cầu đăng nhập; hiện GitHub yêu cầu token thay password — bạn có thể dùng GitHub Desktop để tránh phiền phức (xem bên dưới).

#### Cách 2 — Dùng GitHub Web (nếu ít file)
1. Vào repo trên GitHub → **Add file → Upload files**.
2. Kéo toàn bộ **nội dung thư mục** (không upload file zip) vào ô upload.
3. Commit changes. (Lưu ý: web upload có thể thất bại nếu số lượng file lớn hoặc kích thước vượt giới hạn.)

**Gợi ý:** nếu không quen lệnh, dùng **GitHub Desktop** (https://desktop.github.com) — cài đặt, đăng nhập, kéo folder vào ứng dụng, commit và push rất dễ.

---

### C. Sau khi đã push lên `main`
1. Vào repository trên GitHub → tab **Actions**.
2. Bạn sẽ thấy workflow `Build Electron App (Windows)` đang chạy (hoặc bạn có thể kích hoạt thủ công bằng **Run workflow** nếu cần).
3. Chờ khoảng 5–15 phút (tùy kích thước và tải). Khi hoàn tất, click vào workflow run → cuối trang sẽ có phần **Artifacts** (tên `windows-installer`) — tải file `.exe` (hoặc file installer `.nsis.exe`) về máy.

---

## Một số lưu ý & khắc phục lỗi thường gặp
- Nếu workflow báo lỗi ở bước `Install dependencies`:
  - Kiểm tra `package.json` có hợp lệ không; GitHub Actions dùng Node 18 theo cấu hình.
  - Bạn có thể thử chỉnh lại lệnh `npm install` thành `npm ci` nếu có `package-lock.json`.

- Nếu không thấy artifact sau khi chạy xong:
  - Mở logs từng bước (click vào tên step) và xem `npx electron-builder --win` có tạo ra thư mục `dist/` không.
  - Nếu `dist/` trống, kiểm tra lỗi trong logs.

- Vấn đề chữ ký số (code signing):
  - Electron-builder mặc định sẽ cố gắng code-sign nếu tìm thấy thiết lập. Workflow đã đặt `CSC_IDENTITY_AUTO_DISCOVERY=false` để tránh tự động tìm chữ ký. Ứng dụng sẽ **không được ký** — Windows Defender có thể hiển thị cảnh báo 'Unknown publisher' khi lần đầu cài.

- Nếu upload qua web thất bại vì quá nhiều file → dùng Git hoặc GitHub Desktop.

---

## Muốn mình hỗ trợ thêm?
- Mình có thể cung cấp ảnh chụp màn hình cho từng bước (tạo repo, upload, mở Actions, tải artifact). Nếu bạn muốn, mình sẽ tạo loạt ảnh minh họa.
- Nếu bạn muốn mình **gửi file .exe** trực tiếp, mình không thể do giới hạn môi trường. Tuy nhiên quy trình GitHub Actions cho phép bạn tự động nhận file .exe mà không cần tự build trên máy.

Chúc bạn thành công! Nếu bạn muốn, hãy gửi cho mình link repo khi upload xong — mình sẽ kiểm tra workflow và hướng dẫn sửa lỗi nếu có.
