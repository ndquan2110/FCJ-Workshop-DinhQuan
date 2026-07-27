---
title: "Worklog Tuần 2"
date: 2026
weight: 2
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
| 1   | - Tìm hiểu và triển khai Amazon RDS tích hợp cùng ứng dụng Node.js.<br> - - Khởi tạo cơ sở dữ liệu RDS MySQL nằm trong private subnet, cấu hình DB subnet group và thiết lập kết nối từ EC2 web server.                                                 | 24/04/2026 | 26/04/2026      | <https://000005.awsstudygroup.com/> |
| 2   | - Áp dụng kỹ thuật security group chaining để nâng cao bảo mật cho cơ sở dữ liệu.<br> - Cấu hình chỉ cho phép lưu lượng (traffic) MySQL bắt nguồn từ EC2 Web App Security Group, tuyệt đối không mở trực tiếp database ra Internet.<br>                                              | 25/04/2026 | 26/04/2026      | <https://000005.awsstudygroup.com/><https://000003.awsstudygroup.com/> |
| 3   | - Thực hành chiến lược gắn thẻ tài nguyên (Resource Tagging).<br> - - Khởi chạy các máy ảo EC2 phục vụ cho những môi trường khác nhau và thực hiện quản lý tag hàng loạt trực tiếp trên giao diện EC2 Tags console.<br>  | 27/04/2026   | 28/04/2026      | <https://000027.awsstudygroup.com/> |
| 4   | - Xây dựng Resource Group dựa trên hệ thống tag. <br> - Xem trước (preview) và lưu lại các nhóm tài nguyên theo điều kiện lọc tag của EC2 nhằm hỗ trợ tìm kiếm, quản lý và vận hành hiệu quả hơn.<br>                            | 28/04/2026   | 29/04/2026      | <https://000027.awsstudygroup.com/> |



### Kết quả đạt được tuần 2:

* Tổng quan:

Trong tuần này, tôi tập trung vào chủ đề rds, gắn thẻ tài nguyên và nền tảng triển khai ứng dụng. Nội dung được tổng hợp từ worklog theo ngày và biên tập lại thành định dạng báo cáo theo tuần.

*Kiến thức đã học:

- Chuyển từ mô hình database cục bộ sang kiến trúc managed database trên AWS.
- Thực hành cô lập mạng giữa web server public và database private.
- Sử dụng tag và resource group để tổ chức tài nguyên cloud.
- Chuẩn bị nền tảng cho các bài tự động hóa và CI/CD phía sau.

* Thực hành:

- Triển khai được ứng dụng web cơ bản kết nối Amazon RDS.
- Áp dụng mô hình public/private rõ ràng cho tầng ứng dụng và tầng database.
- Sử dụng tag và resource group để tổ chức tài nguyên, phục vụ các bài governance sau này.



