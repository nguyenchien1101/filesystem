# MyFS – Secure Encrypted File System  
**Data Integrity & Disaster Recovery Project**

## 📌 Giới thiệu
MyFS (My File System) là một hệ thống quản lý dữ liệu dưới dạng **volume mã hóa**, được thiết kế nhằm đảm bảo **tính bảo mật, toàn vẹn và khả năng phục hồi dữ liệu** trong các kịch bản sự cố logic hoặc truy cập trái phép.

Khác với các hệ thống file truyền thống phụ thuộc vào cơ chế bảo vệ của hệ điều hành, MyFS triển khai **một lớp bảo mật độc lập ở mức ứng dụng**, kết hợp mã hóa mạnh, xác thực đa yếu tố và ràng buộc thiết bị.

Dự án được thực hiện trong khuôn khổ môn học:  
**An toàn dữ liệu, khôi phục thông tin sau sự cố (Data Integrity & Disaster Recovery)**.

---

## 🎯 Mục tiêu hệ thống
MyFS hướng tới 3 mục tiêu cốt lõi:

- **Confidentiality**: Bảo mật dữ liệu ngay cả khi file volume bị đánh cắp
- **Integrity**: Phát hiện mọi thay đổi trái phép trên dữ liệu
- **Disaster Recovery**: Hỗ trợ phục hồi dữ liệu khi xóa nhầm hoặc lỗi logic

---

## 🧱 Kiến trúc tổng thể

### 1. Mô hình lưu trữ hai thành phần
- **Volume (.DRI)**  
  File duy nhất đóng vai trò như một “ổ đĩa ảo”, chứa toàn bộ dữ liệu ở dạng mã hóa
- **Key file (.key)**  
  Chứa khóa mã hóa chính, khuyến nghị lưu trên thiết bị rời (USB)

➡️ Việc tách khóa khỏi volume giúp giảm rủi ro khi volume bị sao chép trái phép.

---

### 2. Cơ chế bảo mật nhiều lớp
MyFS áp dụng mô hình **Multi-Factor Security**, bao gồm:

1. **Mật khẩu volume**
   - Dẫn xuất khóa bằng Argon2id
2. **Key file (.key)**
   - Chứa khóa mã hóa superblock
3. **OTP (TOTP – 2FA)**
   - Mỗi lần mở volume cần mã OTP từ ứng dụng xác thực
4. **Device Binding**
   - Ràng buộc volume với BIOS UUID của thiết bị tạo volume

➡️ Chống hiệu quả brute-force, đánh cắp file và truy cập từ thiết bị khác.

---

## 🔐 Thiết kế mã hóa & toàn vẹn dữ liệu

### 🔑 Dẫn xuất khóa
- **Argon2id**
  - Chống brute-force
  - Chống tấn công GPU / ASIC

### 🔒 Mã hóa dữ liệu
- **AES-GCM**
  - Đảm bảo bảo mật + xác thực dữ liệu
  - Sử dụng nonce ngẫu nhiên cho mỗi lần mã hóa

### 🧪 Kiểm tra toàn vẹn
- Hash SHA-256 được tính trên dữ liệu đã mã hóa
- Hash được lưu trong volume
- Nếu hash không khớp → volume bị từ chối mở

---

## ♻️ Cơ chế phục hồi dữ liệu

### 1. Soft Delete & Restore
- File bị xóa chỉ được đánh dấu logic
- Có thể khôi phục nếu xác thực đúng

### 2. Permanent Delete
- Dữ liệu bị ghi đè trước khi xóa
- Giảm khả năng phục hồi nội bộ volume

---

## ⚙️ Quy trình vận hành

### 🔹 Tạo volume
1. Tạo file `.DRI`
2. Thiết lập mật khẩu
3. Sinh file `.key`
4. Cấu hình OTP (QR code)

### 🔹 Mở volume
1. Nhập mật khẩu
2. Cung cấp key file
3. Nhập mã OTP
4. Kiểm tra BIOS UUID

### 🔹 Quản lý file
- Import / Export file
- Đổi mật khẩu
- Xóa, khôi phục hoặc xóa vĩnh viễn

---

## 📊 Đánh giá
MyFS đáp ứng tốt yêu cầu của bài toán **Data Integrity & Disaster Recovery**, đặc biệt ở khía cạnh bảo mật dữ liệu. Việc kết hợp mã hóa mạnh, xác thực đa yếu tố và ràng buộc thiết bị tạo nên một mô hình bảo vệ dữ liệu có tính thực tiễn cao.

### 🔧 Hạn chế
- Chưa hỗ trợ snapshot hoặc journal
- Phục hồi dữ liệu còn giới hạn ở mức logic

### 🚀 Hướng phát triển
- Snapshot / journaling
- Versioning file
- Tách quyền người dùng

---

## 👨‍🎓 Thông tin nhóm
- **Lê Quốc Anh** – 22520049  
- **Võ Nguyễn Chiến** – 22520157  

Lớp: **NT212.Q11.ANTT**  
GVHD: **ThS. Thái Hùng Văn**

---

## 🔗 Liên kết
- GitHub repository:  
  https://github.com/nguyenchien1101/filesystem
