---
title: "Cơ sở dữ liệu & Hạ tầng"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3. </b> "
---

Trong phần này, chúng ta sẽ tiến hành thiết lập và cấu hình thủ công các dịch vụ hạ tầng cốt lõi trên **AWS Management Console** bao gồm: **DynamoDB**, **S3 Bucket**, **SQS Queue** và **Amazon Cognito**.

---

### Bước 1: Cài đặt thư viện phụ thuộc (npm packages)

Di chuyển vào thư mục dự án trên máy tính của bạn và cài đặt các thư viện cần thiết cho cả backend và frontend:

```bash
# 1. Di chuyển vào thư mục backend và cài đặt thư viện
cd backend
npm install

# 2. Di chuyển vào thư mục frontend và cài đặt thư viện
cd ../frontend
npm install
```

---

### Bước 2: Khởi tạo các bảng Amazon DynamoDB trên AWS Web Console

Hệ thống quản lý sinh viên sử dụng 6 bảng DynamoDB để lưu trữ thông tin nghiệp vụ độc lập:

* **Students**: Lưu hồ sơ sinh viên (Partition key: `id` - String).
* **Teachers**: Lưu hồ sơ giáo viên (Partition key: `id` - String).
* **Classes**: Lưu danh sách lớp học (Partition key: `id` - String).
* **Grades**: Lưu điểm số môn học (Partition key: `id` - String).
* **Materials**: Lưu tài liệu học tập của giáo viên (Partition key: `id` - String).
* **Documents**: Lưu metadata hồ sơ của sinh viên (Partition key: `id` - String).

#### từng bước thực hiện trên AWS Console:
1. Đăng nhập vào [AWS DynamoDB Console](https://us-east-1.console.aws.amazon.com/dynamodbv2/home?region=us-east-1#tables).
2. Ở thanh điều hướng bên trái, nhấp chọn **Tables** → Nhấp nút **Create table** (Tạo bảng).
3. Nhập **Table name**: `Students`.
4. Nhập **Partition key**: `id`, chọn kiểu dữ liệu là **String**.
5. Trong mục **Table settings**, chọn **Customize settings** → Tại phần **Read/write capacity settings**, chọn **On-demand** (PAY_PER_REQUEST).
6. Cuộn xuống cuối trang và nhấp **Create table**.
7. Lặp lại bước 2 - 6 tương tự cho 5 bảng còn lại: `Teachers`, `Classes`, `Grades`, `Materials`, và `Documents`.
8. Kiểm tra danh sách 6 bảng đã hiển thị ở trạng thái **Active**.

![Tạo bảng DynamoDB trên AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_console_dynamodb_create.png)

![Danh sách các bảng DynamoDB thực tế trên AWS Console](/images/5-Workshop/5.3-Infrastructure-database/dynamodb_tables_console.png)

---

### Bước 3: Tạo S3 Document Bucket & Cấu hình CORS trên AWS Web Console

S3 Bucket dùng để lưu trữ file tài liệu thực tế của sinh viên và giáo viên. Do Frontend React (chạy tại trình duyệt) sẽ tải trực tiếp file lên S3 qua **Presigned URL**, chúng ta cần cấu hình **CORS** (Cross-Origin Resource Sharing) để trình duyệt không chặn request.

#### 📌Từng bước thực hiện trên AWS Console:

##### 1. Tạo S3 Bucket lưu tài liệu
1. Đăng nhập vào [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
2. Nhấp chọn nút **Create bucket** (Tạo bucket).
3. Nhập **Bucket name**: `student-documents-147997148454` *(Tên bucket phải là duy nhất toàn cầu)*.
4. Chọn **AWS Region**: `us-east-1` (US East - N. Virginia).
5. Trong phần **Block Public Access settings for this bucket**, giữ nguyên tùy chọn **Block *all* public access**.
6. Cuộn xuống dưới và nhấp chọn **Create bucket**.

##### 2. Cấu hình quy tắc CORS cho S3 Bucket
1. Tại danh sách Buckets, nhấp chọn vào tên bucket vừa tạo (`student-documents-147997148454`).
2. Chuyển sang tab **Permissions** (Quyền truy cập).
3. Cuộn xuống phần **Cross-origin resource sharing (CORS)** và nhấp chọn **Edit**.
4. Dán đoạn mã cấu hình JSON sau vào trình biên soạn:

```json
[
  {
    "AllowedHeaders": [
      "*"
    ],
    "AllowedMethods": [
      "GET",
      "PUT",
      "POST",
      "DELETE",
      "HEAD"
    ],
    "AllowedOrigins": [
      "*"
    ],
    "ExposeHeaders": [
      "ETag"
    ]
  }
]
```

5. Nhấp chọn **Save changes** để hoàn tất cấu hình CORS.

![Cấu hình S3 Bucket & CORS trên AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_console_s3_create.png)

---

### Bước 4: Tạo SQS Queue cho hệ thống thông báo trên AWS Web Console

Tạo hàng đợi thông báo SQS để trung chuyển sự kiện gửi email bất đồng bộ từ các dịch vụ backend sang Lambda Notification Worker.

#### 📌 Hướng dẫn từng bước thực hiện trên AWS Console:
1. Đăng nhập vào [Amazon SQS Console](https://us-east-1.console.aws.amazon.com/sqs/v2/home?region=us-east-1#/queues).
2. Nhấp chọn **Create queue** (Tạo hàng đợi).
3. Chọn loại hàng đợi: **Standard** (Tiêu chuẩn).
4. Nhập **Name**: `student-notifications`.
5. Giữ nguyên các thông số thời gian mặc định (Visibility timeout: `30 seconds`, Message retention period: `4 days`).
6. Cuộn xuống cuối trang và nhấp chọn **Create queue**.
7. Lưu lại thông tin **Queue URL** hiển thị trên màn hình:
   ```text
   https://sqs.us-east-1.amazonaws.com/147997148454/student-notifications
   ```

![Tạo Amazon SQS Queue trên AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_sqs_create.png)

---

### Bước 5: Thiết lập Amazon Cognito User Pool trên AWS Web Console

Amazon Cognito quản lý việc đăng ký, đăng nhập và cấp mã Token bảo mật JWT để xác thực các request gửi lên API Gateway.

#### 📌 Hướng dẫn từng bước thực hiện trên AWS Console:

##### 1. Tạo Cognito User Pool
1. Đăng nhập vào [Amazon Cognito Console](https://us-east-1.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-east-1).
2. Nhấp chọn nút **Create user pool**.
3. **Step 1 (Configure sign-in experience)**: Tại mục **Provider attributes**, chọn **Email**. Nhấp **Next**.
4. **Step 2 (Configure security requirements)**: Chọn **No MFA** (dành cho môi trường test/demo). Nhấp **Next**.
5. **Step 3 (Configure sign-up experience)**: Giữ mặc định và nhấp **Next**.
6. **Step 4 (Configure message delivery)**: Chọn **Send email with Cognito** (hoặc SES). Nhấp **Next**.
7. **Step 5 (Integrate your app)**:
   * Nhập **User pool name**: `student-portal-user-pool` *(User Pool ID thực tế: `us-east-1_7SwNQ0qYm`)*.
   * Tại mục **App client**, chọn **Public client**.
   * Nhập **App client name**: `student-portal-react-client`.
   * Đảm bảo tùy chọn **Don't generate a client secret** được chọn.
8. **Step 6 (Review and create)**: Kiểm tra lại các thông tin và nhấp **Create user pool**.

##### 2. Tạo các Nhóm người dùng (Groups)
1. Nhấp chọn vào User Pool vừa tạo (`student-portal-user-pool`).
2. Chuyển sang tab **Groups** → Nhấp **Create group**.
3. Tạo tuần tự 3 nhóm với thông số:
   * Nhóm 1: Name = `ADMIN`, Description = `Quản trị viên hệ thống`.
   * Nhóm 2: Name = `TEACHER`, Description = `Cán bộ / Giáo viên giảng dạy`.
   * Nhóm 3: Name = `STUDENT`, Description = `Sinh viên tra cứu học tập`.

##### 3. Tạo tài khoản Admin thử nghiệm
1. Chuyển sang tab **Users** → Nhấp **Create user**.
2. Chọn **Send an email invitation**.
3. Nhập **Email address**: `admin@example.com`.
4. Nhập **Temporary password**: `Abc12345!`.
5. Nhấp **Create user**. Sau đó gán user này vào nhóm `ADMIN`.

![Tạo Amazon Cognito User Pool & Groups trên AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_console_cognito_create.png)

![Màn hình chi tiết Cognito User Pool trên AWS Console](/images/5-Workshop/5.3-Infrastructure-database/cognito_user_pool_console.png)
