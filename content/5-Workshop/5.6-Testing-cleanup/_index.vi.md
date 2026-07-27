---
title: "Kiểm thử & Dọn dẹp"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.6. </b> "
---

Trong phần cuối của workshop, chúng ta sẽ thực hiện giám sát hoạt động hệ thống bằng **Amazon CloudWatch Logs** và tiến hành dọn dẹp các dịch vụ AWS đã khởi tạo để tránh phát sinh chi phí.

---

### 1. Giám sát hệ thống qua CloudWatch

Mỗi khi các hàm Lambda được kích hoạt từ API Gateway hoặc SQS, log của chúng sẽ được ghi nhận tự động tại **Amazon CloudWatch Logs**.

#### 📌 Từng bước kiểm tra log trên AWS Console:
1. Đăng nhập vào [Amazon CloudWatch Console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1#logsV2:log-groups).
2. Chọn **Log groups** ở menu bên trái.
3. Tìm kiếm nhóm log tương ứng với hàm Lambda thực tế: `/aws/lambda/getStudents` hoặc `/aws/lambda/sendEmailWorker`.
4. Chọn log stream mới nhất để theo dõi chi tiết quá trình xử lý, lỗi phát sinh hoặc mã trạng thái trả về.

![Kiểm thử & Giám sát CloudWatch Logs](/images/5-Workshop/5.6-Testing-cleanup/postman_cloudwatch_testing.png)

---

### 2. Dọn dẹp tài nguyên trên AWS (Cleanup)

> [!CAUTION]
> Để tránh phát sinh chi phí không mong muốn trên tài khoản AWS của bạn, hãy chắc chắn thực hiện đầy đủ các bước dọn dẹp dưới đây trên AWS Web Console sau khi đã hoàn thành bài tập hoặc báo cáo.

#### 📌 Dọn dẹp từng dịch vụ trên AWS Web Console:

##### 1. Xóa Amazon CloudFront Distribution (Nếu có)
1. Đăng nhập vào [Amazon CloudFront Console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
2. Chọn Distribution `E39TFB7INWHA6Y` → Nhấp chọn **Disable**.
3. Chờ trạng thái chuyển sang **Disabled** (khoảng 3-5 phút), chọn lại Distribution và nhấp **Delete**.

![Xóa CloudFront Distribution trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_cloudfront.jpeg)

##### 2. Xóa Amazon S3 Buckets
1. Đăng nhập vào [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
2. Chọn Bucket `student-documents-147997148454` → Nhấp chọn **Empty** (Xóa toàn bộ đối tượng bên trong) → Xác nhận xóa.
3. Nhấp chọn **Delete** để xóa bucket.
4. Lặp lại các thao tác trên cho S3 Frontend Bucket `student-portal-frontend-147997148454`.

![Xóa S3 Buckets trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_s3.jpeg)

##### 3. Xóa Amazon API Gateway
1. Đăng nhập vào [Amazon API Gateway Console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
2. Chọn REST API `student-portal-api` (`9k9i3ukwdh`) → Nhấp chọn menu **Actions** → Chọn **Delete**.

![Xóa REST API Gateway trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_api.jpeg)

##### 4. Xóa các hàm AWS Lambda
1. Đăng nhập vào [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
2. Tích chọn các hàm Lambda đã tạo (ví dụ: `getStudents`, `createStudent`, `sendEmailWorker`,...) → Nhấp **Actions** → Chọn **Delete**.

![Xóa tất cả các hàm AWS Lambda trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_lambda.jpeg)

##### 5. Xóa Amazon Cognito User Pool
1. Đăng nhập vào [Amazon Cognito Console](https://us-east-1.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-east-1).
2. Chọn User Pool `student-portal-user-pool` (`us-east-1_7SwNQ0qYm`) → Nhấp chọn nút **Delete pool** → Nhập tên User Pool để xác nhận xóa.

![Xóa Cognito User Pool trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_cognito.jpeg)

##### 6. Xóa Amazon SQS Queue
1. Đăng nhập vào [Amazon SQS Console](https://us-east-1.console.aws.amazon.com/sqs/v2/home?region=us-east-1#/queues).
2. Chọn Queue `student-notifications` → Nhấp chọn nút **Delete**.

![Xóa SQS Queue trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_sqs.jpeg)

##### 7. Xóa các bảng Amazon DynamoDB
1. Đăng nhập vào [Amazon DynamoDB Console](https://us-east-1.console.aws.amazon.com/dynamodbv2/home?region=us-east-1#tables).
2. Tích chọn các bảng đã tạo (`Students`, `Teachers`, `Grades`, `Materials`, `Documents`, `Classes`) → Nhấp chọn nút **Delete tables**.

![Xóa các bảng DynamoDB trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_database.jpeg)

##### 8. Xóa IAM Role
1. Đăng nhập vào [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles).
2. Tìm kiếm role `student-portal-lambda` → Tích chọn và nhấp chọn **Delete**.

![Xóa IAM Role trên AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_iam.jpeg)

---

🎉 **Chúc mừng bạn đã hoàn thành bài Lab triển khai Hệ thống Quản lý Sinh viên Serverless trên AWS!**
