---
title: "Triển khai Frontend & Hosting"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.5. </b> "
---

Trong phần này, chúng ta sẽ tìm hiểu cấu trúc các trang giao diện của **AWS Student Management Portal**, cấu hình biến môi trường kết nối tới AWS Cognito và API Gateway, chạy thử ứng dụng ở môi trường local và cuối cùng là deploy đóng gói lên dịch vụ **Amazon S3** và phân phối qua **Amazon CloudFront**.

---

### Bước 1: Cấu trúc và Phân quyền các trang Frontend

Mã nguồn Frontend được phát triển bằng **ReactJS** kết hợp công cụ build **Vite**.

![Giao diện Cổng quản lý sinh viên AWS Student Management Portal](/images/5-Workshop/5.5-Frontend-deployment/student_portal_app_ui.png)

* **Định tuyến bảo vệ (Protected Routing)**: Hệ thống sử dụng component `ProtectedRoute` để kiểm tra JWT Token từ Cognito.
* **Phân quyền dựa trên Nhóm (Group-based authorization)**:
  * **Admin/Staff**: Có quyền truy cập đầy đủ tất cả các trang bao gồm Dashboard, quản lý danh sách học sinh/giáo viên/điểm, xem logs hệ thống và cấu hình cài đặt.
  * **Teacher (Giáo viên)**: Chỉ có quyền xem danh sách lớp học được phân công, đăng/cập nhật điểm số cho học sinh và đăng tải tài liệu học tập.
  * **Student (Sinh viên)**: Chỉ có quyền xem hồ sơ cá nhân, xem điểm số của chính mình, tải tài liệu học tập do giáo viên đăng tải.

---

### Bước 2: Cấu hình biến môi trường Frontend (.env)

Để ứng dụng React biết được thông tin kết nối tới các dịch vụ AWS bạn vừa tạo ở các bước trước, hãy tạo file `frontend/.env` trong thư mục `frontend/` của dự án với nội dung cấu hình thực tế sau:

```env
# AWS Cognito User Pool Configuration
VITE_USER_POOL_ID=us-east-1_7SwNQ0qYm
VITE_APP_CLIENT_ID=student-portal-react-client

# API Gateway Endpoint (Đường dẫn Invoke URL thực tế)
VITE_API_ENDPOINT=https://9k9i3ukwdh.execute-api.us-east-1.amazonaws.com/prod

# AWS Region chạy dịch vụ
VITE_AWS_REGION=us-east-1
```

---

### Bước 3: Chạy thử nghiệm ở môi trường Local

Sau khi đã hoàn tất cấu hình file `.env`, bạn có thể khởi chạy server phát triển local để kiểm tra giao diện:

```bash
cd frontend
npm run dev
```

Mở trình duyệt và truy cập địa chỉ [http://localhost:1313](http://localhost:1313). 

Tại đây, bạn thử nghiệm đăng nhập bằng tài khoản Admin demo đã được tạo tự động:
* **Email**: `admin@example.com`
* **Mật khẩu**: `Abc12345!`

Trong lần đăng nhập đầu tiên, hệ thống sẽ yêu cầu bạn đổi mật khẩu mới để tăng tính bảo mật.

![Giao diện Quản trị trang Admin của Student Portal](/images/5-Workshop/5.5-Frontend-deployment/student_portal_admin.jpeg)

---

### Bước 4: Tạo S3 Frontend Bucket & Upload mã nguồn trên AWS Web Console

Khi ứng dụng đã chạy ổn định ở local, chúng ta tiến hành đóng gói mã nguồn React thành các file HTML/JS/CSS tĩnh để đưa lên AWS hosting.

#### 📌 Từng bước thực hiện trên AWS Console:
1. Mở Terminal tại máy tính cá nhân và chạy lệnh đóng gói:
   ```bash
   cd frontend
   npm run build
   ```
   Lệnh này sẽ tạo ra một thư mục tên là `dist/` chứa toàn bộ code tĩnh của trang web.

2. Đăng nhập vào [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
3. Nhấp chọn nút **Create bucket** (Tạo bucket).

![Khởi tạo S3 Frontend Bucket](/images/5-Workshop/5.5-Frontend-deployment/create_bucket_s3.png)

4. Nhập **Bucket name**: `student-portal-frontend-147997148454` *(Tên bucket duy nhất toàn cầu)*.
5. Chọn **AWS Region**: `us-east-1` (US East - N. Virginia).
6. Trong phần **Block Public Access settings for this bucket**, giữ nguyên tùy chọn **Block *all* public access** để bảo vệ đĩa lưu trữ, chỉ cho phép CloudFront truy cập thông qua Origin Access Control (OAC).
7. Cuộn xuống cuối trang và nhấp chọn **Create bucket**.
8. Nhấp chọn vào tên bucket `student-portal-frontend-147997148454` vừa tạo → Nhấp chọn nút **Upload**.
9. Kéo thả toàn bộ các file và thư mục bên trong `dist/` vào khung tải lên và nhấp **Upload**.

![Upload các file giao diện tĩnh lên S3 Bucket](/images/5-Workshop/5.5-Frontend-deployment/Upload_bucket.png)

---

### Bước 5: Khởi tạo & Phân phối qua Amazon CloudFront trên AWS Web Console

Để tối ưu hóa tốc độ tải trang toàn cầu và hỗ trợ giao thức bảo mật HTTPS, chúng ta triển khai một dịch vụ **Amazon CloudFront Distribution** trỏ vào S3 bucket frontend ở trên làm Origin.

#### 📌 Từng bước thực hiện trên AWS Console:
1. Đăng nhập vào [Amazon CloudFront Console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
2. Nhấp chọn **Create distribution** (Tạo bản phân phối).
3. Tại ô **Origin domain**, chọn S3 Bucket frontend của bạn (`student-portal-frontend-147997148454.s3.amazonaws.com`).
4. Tại mục **Origin access**, chọn **Origin access control settings (recommended)** (OAC) → Nhấp **Create control setting** → Giữ mặc định và nhấp **Create**.
5. Trong phần **Default cache behavior**:
   - Tại **Viewer protocol policy**, chọn **Redirect HTTP to HTTPS**.
   - Tại **Allowed HTTP methods**, chọn **GET, HEAD**.
6. Tại phần **Settings**:
   - Nhập **Default root object**: `index.html`.
7. Nhấp chọn **Create distribution**.
8. Copy đoạn **S3 bucket policy** do CloudFront tự động tạo ra và dán vào tab **Permissions** > **Bucket policy** của S3 Frontend Bucket để cấp quyền cho CloudFront.

![Cập nhật S3 Bucket Policy cấp quyền cho CloudFront](/images/5-Workshop/5.5-Frontend-deployment/Upload_bucket.png)

9. Chờ quá trình phân phối hoàn tất (trạng thái hiển thị **Enabled** - khoảng 3-5 phút), thu được tên miền CloudFront chính thức của sản phẩm:
   ```text
   https://d3th0yl82lu593.cloudfront.net/
   ```

![Chi tiết Amazon CloudFront Distribution E39TFB7INWHA6Y](/images/5-Workshop/5.5-Frontend-deployment/CloudFront_Distributions_E39TFB7INWHA6Y.png)

---

### Bước 6: Danh sách tài khoản thử nghiệm hệ thống

| Nhóm quyền | Email đăng nhập | Mật khẩu | Chức năng quan sát |
|------------|-----------------|----------|-------------------|
| **Admin** | `admin@example.com` | `Abc12345!` | Thấy toàn bộ chức năng (gồm quản lý giáo viên, log, tài khoản). |
| **Giáo viên / Cán bộ** | `staff@example.com` | `Abc12345!` | Thấy menu Sinh viên, Điểm số, Tài liệu học tập, Hồ sơ, Thông báo. (Ẩn phần cấu hình Admin và Giáo viên). |
| **Sinh viên** | `student@example.com` | `Abc12345!` | Chỉ thấy menu Tổng quan, Hồ sơ cá nhân, Xem điểm số, Tài liệu học tập và Thông báo. (Ẩn toàn bộ mục quản lý còn lại). |

![Giao diện Quản trị trang Admin của Student Portal](/images/5-Workshop/5.5-Frontend-deployment/student_portal_admin.jpeg)

![Giao diện Quản trị trang staff của Student Portal](/images/5-Workshop/5.5-Frontend-deployment/student_portal_staff.jpeg)

![Giao diện Quản trị trang student của Student Portal](/images/5-Workshop/5.5-Frontend-deployment/student_portal_student.jpeg)