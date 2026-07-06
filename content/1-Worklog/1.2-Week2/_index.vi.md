---
title: "Worklog Tuần 2"
date: 2026
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:

* Chuyển từ mô hình database cục bộ sang kiến trúc managed database trên AWS.
* Thực hành cô lập mạng giữa web server public và database private.
* Sử dụng tag và resource group để tổ chức tài nguyên cloud.
* Chuẩn bị nền tảng cho các bài tự động hóa và CI/CD phía sau.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1   | - Học Amazon RDS với ứng dụng Node.js.<br> - Tạo RDS MySQL trong private subnet, cấu hình DB subnet group và kết nối từ EC2 web server.                                                                                       | 24/04/2026   | 26/04/2026      |
| 2   | - Áp dụng security group chaining để bảo vệ database.
- Chỉ cho phép traffic MySQL đi từ EC2 Web App Security Group và tránh mở database ra Internet.                                         | 25/04/2026   | 26/04/2026      | [<https://cloudjourney.awsstudygroup.com/>](https://000005.awsstudygroup.com/) |
| 3   | - Thực hành chiến lược gắn thẻ tài nguyên.
- Tạo EC2 cho các môi trường khác nhau và quản lý tag hàng loạt qua EC2 Tags console. | 26/04/2026   | 28/04/2026      | [<https://cloudjourney.awsstudygroup.com/>](https://000027.awsstudygroup.com/) |
| 4   | - Tạo Resource Group dựa trên tag.
- Preview và lưu nhóm tài nguyên theo điều kiện tag của EC2 để dễ tìm kiếm và quản lý.                  | 28/04/2026   | 30/04/2026      | [<https://cloudjourney.awsstudygroup.com/>](https://000027.awsstudygroup.com/) |


### Kết quả đạt được tuần 2:

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản: 
  * Compute
  * Storage
  * Networking 
  * Database
  * ...

* Đã tạo và cấu hình AWS Free Tier account thành công.

* Làm quen với AWS Management Console và biết cách tìm, truy cập, sử dụng dịch vụ từ giao diện web.

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key
  * Secret Key
  * Region mặc định
  * ...

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:

  * Kiểm tra thông tin tài khoản & cấu hình
  * Lấy danh sách region
  * Xem dịch vụ EC2
  * Tạo và quản lý key pair
  * Kiểm tra thông tin dịch vụ đang chạy
  * ...

* Có khả năng kết nối giữa giao diện web và CLI để quản lý tài nguyên AWS song song.
* ...



