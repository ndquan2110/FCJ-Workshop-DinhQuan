---
title: "Week 12 Worklog"
date: 2024-01-01
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---


## Week 12 Objectives

In week 12, the main goal was to apply the AWS knowledge learned to the project implementation process. The content focuses on analyzing system requirements, selecting appropriate AWS services, designing the overall architecture, and deploying the main components of the project on the AWS environment.

The project uses services such as **Amazon S3, CloudFront, Cognito, API Gateway, Lambda, DynamoDB, SQS, SNS, SES, and CloudWatch** to build a serverless-oriented system capable of storing data, authenticating users, processing business logic, sending notifications, and monitoring system activity.

---

## Tasks to be carried out this week

| No. | Task                                                                                                                                                                                               | Start Date | Completion Date | Reference Material                                                                                                                                                                                                                                   |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **1** | Researched and analyzed project requirements; identified core system features; and selected optimal AWS services for deployment.                                                  | 05/06/2026 | 05/06/2026      | [https://cloudjourney.awsstudygroup.com/vi/1-explore/](https://cloudjourney.awsstudygroup.com/vi/1-explore/) <br> [https://cloudjourney.awsstudygroup.com/vi/4-modernize/](https://cloudjourney.awsstudygroup.com/vi/4-modernize/)               |
| **2** | Designed the overall system architecture, outlining the end-to-end operational workflow among end-users, the frontend tier, API Gateway, AWS Lambda, and the database.                                                          | 06/06/2026 | 10/06/2026      | [https://000079.awsstudygroup.com/vi/](https://000079.awsstudygroup.com/vi/) <br> [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/)                                                                                   |
| **3** | Implemented storage solutions and user authentication mechanisms, including hosting the frontend/files on Amazon S3, content distribution via CloudFront, and centralized authentication management with Amazon Cognito.                                              | 11/06/2026 | 15/06/2026      | [https://000057.awsstudygroup.com/vi/](https://000057.awsstudygroup.com/vi/) <br> [https://000081.awsstudygroup.com/vi/](https://000081.awsstudygroup.com/vi/)                                                                                   |
| **4** | Developed the serverless backend tier using API Gateway integrated with AWS Lambda; established database connectivity between Lambda and DynamoDB to handle CRUD operations (Create, Read, Update, Delete) and data queries.                                              | 16/06/2026 | 20/06/2026      | [https://000079.awsstudygroup.com/vi/](https://000079.awsstudygroup.com/vi/) <br> [https://000078.awsstudygroup.com/vi/](https://000078.awsstudygroup.com/vi/) <br> [https://000053.awsstudygroup.com/vi/](https://000053.awsstudygroup.com/vi/) |
| **5** |Integrated automated notification and email messaging systems via SQS, SNS, and SES; and configured centralized log monitoring and application error tracking using CloudWatch.                                                                                        | 20/06/2026 | 25/06/2026      | [https://000083.awsstudygroup.com/vi/][https://000008.awsstudygroup.com/vi/] |      |

---

## Week 12 Achievements

### Overview

This week, I began applying AWS knowledge to a real project. Instead of exploring individual services separately, I focused on combining AWS services to build a complete system. The services were selected according to their roles in the architecture, including frontend storage, user authentication, API processing, data storage, notifications, and system monitoring.
