---
title: "Worklog Tuần 12"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---




## Mục tiêu tuần 12

Trong tuần 12, mục tiêu chính là áp dụng các kiến thức AWS đã học vào quá trình thực hiện dự án. Nội dung tập trung vào việc phân tích yêu cầu hệ thống, lựa chọn các dịch vụ AWS phù hợp, thiết kế kiến trúc tổng thể và triển khai các thành phần chính của dự án trên môi trường AWS.

Dự án sử dụng các dịch vụ như **Amazon S3, CloudFront, Cognito, API Gateway, Lambda, DynamoDB, SQS, SNS, SES và CloudWatch** để xây dựng hệ thống theo hướng serverless, có khả năng lưu trữ dữ liệu, xác thực người dùng, xử lý nghiệp vụ, gửi thông báo và giám sát hoạt động hệ thống.

---

## Các công việc cần triển khai trong tuần này

| Thứ tự | Công việc thực hiện                                                                                                                    | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                   |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1**  | Nghiên cứu và phân tích yêu cầu bài toán dự án; xác định các tính năng trọng tâm của hệ thống; đồng thời lựa chọn các dịch vụ AWS tối ưu để triển khai.                | 05/06/2026   | 05/06/2026      | [https://cloudjourney.awsstudygroup.com/vi/1-explore/](https://cloudjourney.awsstudygroup.com/vi/1-explore/) <br> [https://cloudjourney.awsstudygroup.com/vi/4-modernize/](https://cloudjourney.awsstudygroup.com/vi/4-modernize/)               |
| **2**  | Thiết kế tổng quan kiến trúc dự án, mô tả luồng vận hành xuyên suốt giữa người dùng, tầng giao diện frontend, API Gateway, AWS Lambda và hệ thống cơ sở dữ liệu.       | 06/06/2026   | 10/06/2026      | [https://000079.awsstudygroup.com/vi/](https://000079.awsstudygroup.com/vi/) <br> [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/)                                                                                   |
| **3**  | Xây dựng giải pháp lưu trữ và cơ chế định danh, xác thực người dùng; bao gồm việc lưu trữ frontend/file trên Amazon S3, phân phối nội dung qua CloudFront và quản lý xác thực tập trung với Amazon Cognito. | 11/06/2026   | 15/06/2026      | [https://000057.awsstudygroup.com/vi/](https://000057.awsstudygroup.com/vi/) <br> [https://000081.awsstudygroup.com/vi/](https://000081.awsstudygroup.com/vi/)                                                                                   |
| **4**  | Phát triển tầng backend theo mô hình serverless sử dụng API Gateway kết hợp AWS Lambda; thiết lập kết nối giữa Lambda và cơ sở dữ liệu DynamoDB nhằm xử lý các thao tác thêm, sửa, xóa và truy vấn dữ liệu.     | 16/06/2026   | 20/06/2026      | [https://000079.awsstudygroup.com/vi/](https://000079.awsstudygroup.com/vi/) <br> [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/) <br> [https://000053.awsstudygroup.com/vi/](https://000053.awsstudygroup.com/vi/) |
| **5**  | Tích hợp hệ thống thông báo và gửi email tự động thông qua SQS, SNS và SES; đồng thời thiết lập hệ thống giám sát log và xử lý lỗi ứng dụng tập trung bằng CloudWatch.                         | 20/06/2026   | 25/06/2026      | [https://000083.awsstudygroup.com/vi/] [https://000008.awsstudygroup.com/vi/]           |

---

## Kết quả đạt được tuần 12

### Tổng quan

Trong tuần này, tôi đã bắt đầu áp dụng kiến thức AWS vào dự án thực tế. Thay vì chỉ tìm hiểu từng dịch vụ riêng lẻ, tôi tập trung vào việc kết hợp các dịch vụ AWS để xây dựng một hệ thống hoàn chỉnh. Các dịch vụ được lựa chọn theo đúng vai trò trong kiến trúc, bao gồm lưu trữ frontend, xác thực người dùng, xử lý API, lưu dữ liệu, gửi thông báo và giám sát hệ thống.

### Kiến thức đã áp dụng

Sau khi thực hiện tuần 12, tôi đã áp dụng được:

* Sử dụng **Amazon S3** để lưu trữ frontend và file của hệ thống.
* Sử dụng **Amazon CloudFront** để phân phối website, tăng tốc truy cập và cải thiện hiệu năng.
* Sử dụng **Amazon Cognito** để xác thực người dùng, hỗ trợ đăng ký, đăng nhập và cấp token.
* Sử dụng **Amazon API Gateway** để tạo REST API và làm điểm giao tiếp giữa frontend với backend.
* Sử dụng **AWS Lambda** để xử lý logic nghiệp vụ theo mô hình serverless.
* Sử dụng **Amazon DynamoDB** để lưu trữ dữ liệu chính của hệ thống.
* Sử dụng **Amazon SQS** để xử lý tác vụ bất đồng bộ.
* Sử dụng **Amazon SNS** và **Amazon SES** để gửi thông báo hoặc email cho người dùng.
* Sử dụng **Amazon CloudWatch** để theo dõi log, kiểm tra lỗi và hỗ trợ troubleshooting.

