---
title: "Worklog Tuần 9"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---



## Mục tiêu tuần 9

Trong tuần 9, mục tiêu chính là tìm hiểu các dịch vụ **giám sát, ghi log và theo dõi hoạt động hệ thống trên AWS**, bao gồm **Amazon CloudWatch** và **AWS CloudTrail**. Đây là các dịch vụ quan trọng giúp người quản trị theo dõi hiệu năng tài nguyên, phát hiện lỗi, tạo cảnh báo và kiểm tra lịch sử hoạt động trong tài khoản AWS.

Nội dung tuần này thuộc nhóm **Optimize / Tối ưu hệ thống trên AWS**, tập trung vào vận hành, giám sát, bảo mật, hiệu năng và tối ưu hệ thống sau khi triển khai lên AWS. ([Cloud Journey][1])

Các nội dung trọng tâm của tuần bao gồm:

* Tìm hiểu tổng quan về giám sát hệ thống trên AWS.
* Tìm hiểu **Amazon CloudWatch Metrics**.
* Tìm hiểu **Amazon CloudWatch Logs** và **CloudWatch Logs Insights**.
* Tìm hiểu cách tạo **CloudWatch Alarm**.
* Tìm hiểu cách tạo **CloudWatch Dashboard**.
* Tìm hiểu **AWS CloudTrail** để ghi nhận lịch sử API call và hoạt động người dùng.
* Phân biệt vai trò của CloudWatch và CloudTrail trong quá trình vận hành hệ thống.

---

## Các công việc cần triển khai trong tuần này

| Thứ tự | Công việc thực hiện                                                                                                                                                 | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --- | --- | --- | --- | --- |
| **1**  | Tìm hiểu tổng quan về giám sát hệ thống trên AWS, vai trò của CloudWatch và CloudTrail trong vận hành, troubleshooting và bảo mật hệ thống.                         | 22/05/2026 | 22/05/2026 | [https://cloudjourney.awsstudygroup.com/vi/3-optimize/](https://cloudjourney.awsstudygroup.com/vi/3-optimize/) <br> [https://000008.awsstudygroup.com/vi/](https://000008.awsstudygroup.com/vi/) <br> [https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html](https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html)                                                                                                                                                                                                           |
| **2**  | Tìm hiểu CloudWatch Metrics, cách xem metric của EC2, RDS, EBS hoặc các dịch vụ AWS khác; tìm hiểu namespace, dimension và statistic.                               | 22/05/2026 | 22/05/2026 | [https://000008.awsstudygroup.com/vi/3-cloud-watch-metric/](https://000008.awsstudygroup.com/vi/3-cloud-watch-metric/) <br> [https://000008.awsstudygroup.com/vi/3-cloud-watch-metric/3.1-viewing-metrics/](https://000008.awsstudygroup.com/vi/3-cloud-watch-metric/3.1-viewing-metrics/) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)                                                                                                                                                   |
| **3**  | Tìm hiểu CloudWatch Logs, Log Group, Log Stream, Logs Insights và cách sử dụng log để hỗ trợ kiểm tra lỗi trong hệ thống.                                           | 23/05/2026 | 23/05/2026 | [https://000008.awsstudygroup.com/vi/4-cloud-watch-log/](https://000008.awsstudygroup.com/vi/4-cloud-watch-log/) <br> [https://000008.awsstudygroup.com/vi/4-cloud-watch-log/4.1-cloud-watch-logs/](https://000008.awsstudygroup.com/vi/4-cloud-watch-log/4.1-cloud-watch-logs/) <br> [https://000008.awsstudygroup.com/vi/4-cloud-watch-log/4.2-cloud-watch-logs-insights/](https://000008.awsstudygroup.com/vi/4-cloud-watch-log/4.2-cloud-watch-logs-insights/)                                                                                                                                                                     |
| **4**  | Tìm hiểu CloudWatch Alarm và CloudWatch Dashboard; thực hành tạo cảnh báo theo ngưỡng metric và tạo dashboard để quan sát tài nguyên.                               | 24/05/2026 | 24/05/2026 | [https://000008.awsstudygroup.com/vi/5-cloud-watch-alarm/](https://000008.awsstudygroup.com/vi/5-cloud-watch-alarm/) <br> [https://000008.awsstudygroup.com/vi/6-cloud-watch-dashboard/](https://000008.awsstudygroup.com/vi/6-cloud-watch-dashboard/) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) |
| **5**  | Tìm hiểu AWS CloudTrail, cách CloudTrail ghi lại hoạt động API trong tài khoản AWS; so sánh CloudWatch và CloudTrail, tổng hợp kiến thức và ghi chú lỗi thường gặp. | 25/05/2026 | 25/05/2026 | [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) <br> [https://aws.amazon.com/cloudtrail/](https://aws.amazon.com/cloudtrail/) <br> [https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html](https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html) <br> [https://cloudjourney.awsstudygroup.com/vi/3-optimize/](https://cloudjourney.awsstudygroup.com/vi/3-optimize/)                           |

---

## Kết quả đạt được tuần 9

### Tổng quan

Trong tuần này, tôi đã tìm hiểu các dịch vụ phục vụ cho việc **giám sát và ghi nhận hoạt động hệ thống trên AWS**. Amazon CloudWatch được sử dụng để thu thập metric, log, tạo alarm và dashboard; trong khi đó AWS CloudTrail tập trung vào việc ghi lại hoạt động người dùng và API call trong tài khoản AWS. Theo tài liệu AWS, CloudWatch phục vụ theo dõi hiệu năng và trạng thái vận hành, còn CloudTrail phục vụ audit, bảo mật và kiểm tra lịch sử hoạt động. <[AWS Documentation][2]>
