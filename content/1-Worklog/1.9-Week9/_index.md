---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---



## Week 9 Objectives

In week 9, the main goal was to explore AWS services for **system monitoring, logging, and activity tracking**, including **Amazon CloudWatch** and **AWS CloudTrail**. These are important services that help administrators monitor resource performance, detect errors, create alerts, and review the activity history in an AWS account.

This week's content belongs to the **Optimize / Optimizing system on AWS** group, focusing on operations, monitoring, security, performance, and system optimization after deployment to AWS. ([Cloud Journey][1])

Key topics for the week include:

* Getting an overview of system monitoring on AWS.
* Exploring **Amazon CloudWatch Metrics**.
* Exploring **Amazon CloudWatch Logs** and **CloudWatch Logs Insights**.
* Learning how to create a **CloudWatch Alarm**.
* Learning how to create a **CloudWatch Dashboard**.
* Exploring **AWS CloudTrail** to record API call history and user activity.
* Distinguishing the roles of CloudWatch and CloudTrail in system operations.

---

## Tasks to be carried out this week

| No. | Task                                                                                                                                                          | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --- | --- | --- | --- | --- |
| **1** | Get an overview of system monitoring on AWS and the role of CloudWatch and CloudTrail in operations, troubleshooting, and security.                           | 22/05/2026 | 22/05/2026 | [https://cloudjourney.awsstudygroup.com/3-optimize/](https://cloudjourney.awsstudygroup.com/3-optimize/) <br> [https://000008.awsstudygroup.com/](https://000008.awsstudygroup.com/) <br> [https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html](https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html)                                                                                                                                                                                                                                                                                                                                                                                                            |
| **2** | Explore CloudWatch Metrics, how to view metrics for EC2, RDS, EBS, and other AWS services; explore namespace, dimension, and statistic.                      | 22/05/2026 | 22/05/2026 | [https://000008.awsstudygroup.com/3-cloud-watch-metric/](https://000008.awsstudygroup.com/3-cloud-watch-metric/) <br> [https://000008.awsstudygroup.com/3-cloud-watch-metric/3.1-viewing-metrics/](https://000008.awsstudygroup.com/3-cloud-watch-metric/3.1-viewing-metrics/) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)                                                                                                                                                                                                                                                                                                                                                                                                            |
| **3** | Explore CloudWatch Logs, Log Group, Log Stream, Logs Insights, and how to use logs to support error investigation.                                           | 23/05/2026 | 23/05/2026 | [https://000008.awsstudygroup.com/4-cloud-watch-log/](https://000008.awsstudygroup.com/4-cloud-watch-log/) <br> [https://000008.awsstudygroup.com/4-cloud-watch-log/4.1-cloud-watch-logs/](https://000008.awsstudygroup.com/4-cloud-watch-log/4.1-cloud-watch-logs/) <br> [https://000008.awsstudygroup.com/4-cloud-watch-log/4.2-cloud-watch-logs-insights/](https://000008.awsstudygroup.com/4-cloud-watch-log/4.2-cloud-watch-logs-insights/)                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **4** | Explore CloudWatch Alarm and CloudWatch Dashboard; practice creating threshold-based alerts and dashboards to observe resources.                              | 24/05/2026 | 24/05/2026 | [https://000008.awsstudygroup.com/5-cloud-watch-alarm/](https://000008.awsstudygroup.com/5-cloud-watch-alarm/) <br> [https://000008.awsstudygroup.com/6-cloud-watch-dashboard/](https://000008.awsstudygroup.com/6-cloud-watch-dashboard/) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Alarms.html) <br> [https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)                                                                                                                                                                                                                                                                            |
| **5** | Explore AWS CloudTrail, how it records AWS API activity; compare CloudWatch and CloudTrail, consolidate knowledge, and note common errors.                  | 25/05/2026 | 25/05/2026 | [https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) <br> [https://aws.amazon.com/cloudtrail/](https://aws.amazon.com/cloudtrail/) <br> [https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html](https://docs.aws.amazon.com/decision-guides/latest/cloudtrail-or-cloudwatch/cloudtrail-or-cloudwatch.html) <br> [https://cloudjourney.awsstudygroup.com/3-optimize/](https://cloudjourney.awsstudygroup.com/3-optimize/)                                                                                                                                                                                                                                      |

---

## Week 9 Achievements

### Overview

This week, I explored the services for **monitoring and recording system activity on AWS**. Amazon CloudWatch is used to collect metrics, logs, alarms, and dashboards; meanwhile AWS CloudTrail focuses on recording user activity and API calls in the AWS account. According to AWS documentation, CloudWatch serves performance and operational-state monitoring, while CloudTrail serves auditing, security, and activity-history review. ([AWS Documentation][2])
