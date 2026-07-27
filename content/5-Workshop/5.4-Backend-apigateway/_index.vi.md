---
title: "Triển khai Backend & API Gateway"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4. </b> "
---

Trong phần này, chúng ta sẽ tiến hành thiết lập và cấu hình thủ công toàn bộ hạ tầng Backend trên **AWS Management Console**, bao gồm: Khởi tạo IAM Role, cấu hình các hàm **AWS Lambda** (Node.js) và thiết lập **Amazon API Gateway** tích hợp bộ xác thực **Cognito Authorizer**.

---

### Bước 1: Khởi tạo IAM Role cho Lambda trên AWS Web Console

Trước khi triển khai các hàm Lambda, chúng ta cần tạo một IAM Role để cấp các quyền truy cập tối thiểu cho Lambda thực thi các tác vụ kết nối tới DynamoDB, S3, SQS, SES và CloudWatch Logs.

#### 📌 Từng bước thực hiện trên AWS Console:
1. Đăng nhập vào [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles).
2. Ở thanh điều hướng bên trái, nhấp chọn **Roles** → Nhấp chọn nút **Create role** (Tạo role).
3. **Step 1 (Select trusted entity)**: Chọn **AWS service**, dưới phần Use case chọn **Lambda**. Nhấp **Next**.
4. **Step 2 (Add permissions)**: Tìm kiếm và tích chọn các quyền (Policies) sau:
   - `AWSLambdaBasicExecutionRole` (Quyền ghi log ra CloudWatch Logs)
   - `AmazonDynamoDBFullAccess` (Quyền đọc/ghi dữ liệu trên 6 bảng DynamoDB)
   - `AmazonS3FullAccess` (Quyền đọc/ghi S3 Bucket & tạo Presigned URL)
   - `AmazonSQSFullAccess` (Quyền gửi/nhận message trên SQS Queue)
   - `AmazonSESFullAccess` (Quyền gửi email thông báo qua SES)
5. Nhấp chọn **Next**.
6. **Step 3 (Name, review, and create)**:
   - Nhập **Role name**: `student-portal-lambda`.
   - Kiểm tra lại danh sách các policy đã chọn và nhấp **Create role**.
7. Sau khi tạo xong, nhấp vào tên role `student-portal-lambda` và copy lại chuỗi **ARN**:
   ```text
   arn:aws:iam::147997148454:role/student-portal-lambda
   ```

![Danh sách IAM Roles trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/iam_roles.png)

![Danh sách IAM Users trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/iam_users.png)

---

### Bước 2: Tạo & Cấu hình các hàm AWS Lambda trên AWS Web Console

Hệ thống quản lý sinh viên sử dụng 33 hàm Lambda xử lý các logic nghiệp vụ riêng biệt (CRUD sinh viên, giáo viên, lớp học, điểm số, tạo URL upload tài liệu và tiến trình worker ngầm gửi email).

#### 📌 Từng bước thực hiện trên AWS Console:
1. Đăng nhập vào [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
2. Nhấp chọn nút **Create function** (Tạo hàm).
3. Chọn tùy chọn **Author from scratch** (Xây dựng từ đầu).
4. Nhập **Function name**: `getStudents` *(Lặp lại quy trình cho các hàm khác như: `createStudent`, `docUploadUrl`, `sendEmailWorker`,...)*.
5. Chọn **Runtime**: `Node.js 18.x` (hoặc `Node.js 20.x`).
6. Trong phần **Change default execution role**:
   - Chọn **Use an existing role**.
   - Tại ô **Existing role**, chọn IAM Role vừa tạo ở Bước 1: `student-portal-lambda`.
7. Nhấp chọn **Create function**.
8. **Cấu hình biến môi trường (Environment Variables)**:
   - Chuyển sang tab **Configuration** → Chọn mục **Environment variables** ở menu bên trái.
   - Nhấp chọn **Edit** → Thêm các cặp Key/Value cấu hình:
     - `DOCUMENTS_BUCKET`: `student-documents-147997148454`
     - `NOTIFICATION_QUEUE_URL`: `https://sqs.us-east-1.amazonaws.com/147997148454/student-notifications`
     - `USER_POOL_ID`: `us-east-1_7SwNQ0qYm`
   - Nhấp chọn **Save**.
9. Tại tab **Code**, tải file nén `.zip` mã nguồn của hàm lên và nhấp **Deploy**.

![Tạo hàm AWS Lambda trên AWS Web Console](/images/5-Workshop/5.4-Backend-apigateway/aws_console_lambda_create.png)

![Danh sách các hàm AWS Lambda trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/lambda.jpeg)

---

### Bước 3: Triển khai & Cấu hình REST API Gateway trên AWS Web Console

Để Frontend có thể giao tiếp bảo mật với các hàm Lambda, chúng ta thiết lập **Amazon API Gateway** làm cổng định tuyến API và tích hợp bộ xác thực **Cognito Authorizer**.

#### 📌 Từng bước thực hiện trên AWS Console:

##### 1. Khởi tạo REST API
1. Đăng nhập vào [Amazon API Gateway Console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
2. Nhấp **Create API** → Tại mục **REST API**, nhấp chọn **Build**.
3. Chọn tùy chọn **New API**.
4. Nhập **API name**: `student-portal-api` *(API ID thực tế: `9k9i3ukwdh`)*.
5. Chọn **Endpoint Type**: `Regional`. Nhấp chọn **Create API**.

![Khởi tạo REST API trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/create_rest_api.jpeg)

##### 2. Tạo Cognito Authorizer bảo mật
1. Tại danh sách menu bên trái của API `student-portal-api`, chọn mục **Authorizers**.
2. Nhấp chọn **Create new authorizer**.
3. Nhập **Authorizer name**: `CognitoAuthorizer`.
4. Chọn **Type**: `Cognito`.
5. Tại ô **Cognito User Pool**, chọn User Pool của bạn: `student-portal-user-pool` (`us-east-1_7SwNQ0qYm`).
6. Nhập **Token Source**: `Authorization`. Nhấp chọn **Create authorizer**.

![Tạo Cognito Authorizer bảo mật](/images/5-Workshop/5.4-Backend-apigateway/create_authorizer.jpeg)

##### 3. Tạo Resources & Phương thức HTTP (Methods)
1. Ở menu bên trái, nhấp chọn **Resources**.
2. Nhấp nút **Actions** → Chọn **Create Resource**:
   - Nhập Resource Name: `students` → Resource Path: `/students`. Nhấp **Create Resource**.
3. Chọn tài nguyên `/students` vừa tạo → Nhấp **Actions** → Chọn **Create Method**:
   - Tạo phương thức **GET**: Chọn Integration type `Lambda Function` → Chọn hàm Lambda `getStudents` → Nhấp **Save**.
   - Tạo phương thức **POST**: Chọn Integration type `Lambda Function` → Chọn hàm Lambda `createStudent` → Nhấp **Save**.
4. Lặp lại việc tạo các Resources khác tương tự: `/teachers`, `/grades`, `/documents`, `/materials`.
5. **Bật bảo mật Cognito**: Chọn từng Method (ví dụ: POST `/students`) → Nhấp chọn **Method Request** → Tại mục **Authorization**, chuyển từ `NONE` sang chọn `CognitoAuthorizer` → Nhấp dấu tích xanh để lưu.

![Tạo Resources & Methods trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/create_resource.jpeg)

##### 4. Bật cấu hình CORS & Deploy API Stage `prod`
1. Chọn root resource `/` hoặc tài nguyên con → Nhấp nút **Actions** → Chọn **Enable CORS**.
2. Giữ nguyên mặc định và nhấp chọn **Enable CORS and replace existing CORS headers**.
3. Nhấp chọn **Actions** → Chọn **Deploy API**.
4. Tại hộp thoại **Deployment stage**, chọn **[New Stage]** → Nhập **Stage name**: `prod`. Nhấp **Deploy**.
5. Copy lại đường dẫn **Invoke URL** hiển thị trên trang tổng quan Stage:
   ```text
   https://9k9i3ukwdh.execute-api.us-east-1.amazonaws.com/prod
   ```

![Giao diện quản lý API Gateway & Lambda trên AWS Console](/images/5-Workshop/5.4-Backend-apigateway/apigateway_lambda_console.png)
