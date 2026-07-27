---
title: "Worklog Tuần 11"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---




## Mục tiêu tuần 11

Trong tuần 11, mục tiêu chính là tìm hiểu mô hình **Serverless trên AWS** và các dịch vụ thường được sử dụng để xây dựng backend không cần quản lý máy chủ. Nội dung này thuộc nhóm **Modernize / Hiện đại hóa ứng dụng** trong AWS Cloud Journey, tập trung vào serverless computing, API-first, hệ thống hướng sự kiện, xác thực người dùng và messaging bất đồng bộ. ([Cloud Journey][1])

Các nội dung trọng tâm của tuần bao gồm:

* Tìm hiểu tổng quan về kiến trúc serverless.
* Tìm hiểu **AWS Lambda** để xử lý logic backend.
* Tìm hiểu **Amazon API Gateway** để tạo REST API.
* Tìm hiểu **Amazon Cognito** để xác thực người dùng.
* Tìm hiểu **Amazon SQS** và **Amazon SNS** để xử lý hàng đợi và thông báo.
* Tìm hiểu **Amazon SES** để gửi email tự động trong ứng dụng.
* Tổng hợp mô hình kết hợp Lambda, API Gateway, Cognito, SQS, SNS và SES.

---

## Các công việc cần triển khai trong tuần này

| Thứ tự | Công việc thực hiện                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1**  | Tìm hiểu tổng quan về hiện đại hóa ứng dụng trên AWS, mô hình serverless, API-first, microservices và event-driven architecture.                  | 31/05/2026   | 31/05/2026      | [https://cloudjourney.awsstudygroup.com/vi/4-modernize/](https://cloudjourney.awsstudygroup.com/vi/4-modernize/) <br> [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/)                                                                                                                                                                                                                   |
| **2**  |Tìm hiểu AWS Lambda, cách tạo Lambda Function, runtime, handler, trigger, IAM Role và cách Lambda xử lý logic backend mà không cần quản lý server.| 1/06/2026   | 1/06/2026      | [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/) <br> [https://000078.awsstudygroup.com/vi/2-xu-ly-va-toi-uu-kich-thuoc-anh-tren-aws/](https://000078.awsstudygroup.com/vi/2-xu-ly-va-toi-uu-kich-thuoc-anh-tren-aws/) <br> [https://000066.awsstudygroup.com/](https://000066.awsstudygroup.com/)                                                                                       |
| **3**  |Tìm hiểu Amazon API Gateway, cách tạo API, method, resource, tích hợp API Gateway với Lambda, bật CORS và kiểm tra API bằng Postman hoặc frontend. | 2/06/2026   | 2/06/2026      | [https://000079.awsstudygroup.com/vi/](https://000079.awsstudygroup.com/vi/) <br> [https://000079.awsstudygroup.com/vi/4-thiet-lap-api-gateway/](https://000079.awsstudygroup.com/vi/4-thiet-lap-api-gateway/) <br> [https://000066.awsstudygroup.com/](https://000066.awsstudygroup.com/)                                                                                                                           |
| **4**  | Tìm hiểu Amazon Cognito, User Pool, Identity Pool, luồng đăng ký, đăng nhập, xác thực token và tích hợp Cognito với API/Lambda.                    | 3/06/2026   | 23/06/2026      | [https://000081.awsstudygroup.com/vi/](https://000081.awsstudygroup.com/vi/) <br> [https://000081.awsstudygroup.com/vi/2-create-user-pool/](https://000081.awsstudygroup.com/vi/2-create-user-pool/) <br> [https://000081.awsstudygroup.com/vi/3-create-api-and-lambda-function/](https://000081.awsstudygroup.com/vi/3-create-api-and-lambda-function/)                                                             |
| **5**  | Tìm hiểu SQS, SNS và SES; cách xử lý hàng đợi, gửi thông báo, gửi email tự động và tổng hợp luồng serverless hoàn chỉnh.                           | 4/06/2026   | 4/06/2026      | [https://000083.awsstudygroup.com/vi/](https://000083.awsstudygroup.com/vi/) <br> [https://000083.awsstudygroup.com/vi/2-create-queue-and-sns-topic/](https://000083.awsstudygroup.com/vi/2-create-queue-and-sns-topic/) <br> [https://aws.amazon.com/vi/ses/](https://aws.amazon.com/vi/ses/) <br> [https://docs.aws.amazon.com/ses/latest/dg/Welcome.html](https://docs.aws.amazon.com/ses/latest/dg/Welcome.html) |

---

## Kết quả đạt được tuần 11

### Tổng quan

Trong tuần này, tôi đã tìm hiểu mô hình **serverless application** trên AWS. Một kiến trúc serverless cơ bản có thể sử dụng **AWS Lambda** để xử lý logic, **Amazon API Gateway** để nhận request từ người dùng, **Amazon Cognito** để xác thực, **Amazon DynamoDB/S3** để lưu trữ dữ liệu và các dịch vụ messaging như **SQS/SNS** để xử lý bất đồng bộ. AWS Study Group cũng mô tả backend serverless có thể dùng Lambda, API Gateway, S3, DynamoDB và Cognito để xây dựng ứng dụng hoàn chỉnh. <[000066.awsstudygroup.com][2]>
