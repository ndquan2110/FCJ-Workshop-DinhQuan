---
title: "Testing & Clean up"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.6. </b> "
---

In the final section of this workshop, we will monitor system execution logs using **Amazon CloudWatch Logs** and clean up all deployed AWS resources to avoid unexpected charges.

---

### 1. System Monitoring via CloudWatch

Whenever Lambda functions are triggered via API Gateway or SQS, execution logs are automatically recorded in **Amazon CloudWatch Logs**.

#### 📌 Step-by-Step AWS Console Log Inspection Guide:
1. Log in to the [Amazon CloudWatch Console](https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1#logsV2:log-groups).
2. Select **Log groups** in the left menu.
3. Search for log groups corresponding to your Lambda functions: `/aws/lambda/getStudents` or `/aws/lambda/sendEmailWorker`.
4. Select the latest log stream to view execution details, errors, or HTTP response status codes.

![Testing & CloudWatch Logs Monitoring](/images/5-Workshop/5.6-Testing-cleanup/postman_cloudwatch_testing.png)

---

### 2. Clean Up AWS Resources (Cleanup)

> [!CAUTION]
> To prevent ongoing charges on your AWS account, ensure you perform the following cleanup steps on the AWS Web Console after completing your workshop or report.

#### 📌 Step-by-Step AWS Web Console Cleanup Guide:

##### 1. Delete Amazon CloudFront Distribution (If created)
1. Log in to the [Amazon CloudFront Console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
2. Select Distribution `E39TFB7INWHA6Y` → Click **Disable**.
3. Wait for the status to change to **Disabled** (approx. 3-5 mins), reselect the distribution and click **Delete**.

![Delete CloudFront Distribution on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_cloudfront.jpeg)

##### 2. Delete Amazon S3 Buckets
1. Log in to the [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
2. Select Bucket `student-documents-147997148454` → Click **Empty** (delete all objects) → Confirm deletion.
3. Click **Delete** to delete the bucket.
4. Repeat for the Frontend S3 Bucket `student-portal-frontend-147997148454`.

![Delete S3 Buckets on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_s3.jpeg)

##### 3. Delete Amazon API Gateway
1. Log in to the [Amazon API Gateway Console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
2. Select REST API `student-portal-api` (`9k9i3ukwdh`) → Click **Actions** menu → Select **Delete**.

![Delete REST API Gateway on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_api.jpeg)

##### 4. Delete AWS Lambda Functions
1. Log in to the [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
2. Select created Lambda functions (`getStudents`, `createStudent`, `sendEmailWorker`, etc.) → Click **Actions** → Select **Delete**.

![Delete all AWS Lambda Functions on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_lambda.jpeg)

##### 5. Delete Amazon Cognito User Pool
1. Log in to the [Amazon Cognito Console](https://us-east-1.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-east-1).
2. Select User Pool `student-portal-user-pool` (`us-east-1_7SwNQ0qYm`) → Click **Delete pool** → Enter User Pool name to confirm.

![Delete Cognito User Pool on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_cognito.jpeg)

##### 6. Delete Amazon SQS Queue
1. Log in to the [Amazon SQS Console](https://us-east-1.console.aws.amazon.com/sqs/v2/home?region=us-east-1#/queues).
2. Select Queue `student-notifications` → Click **Delete**.

![Delete SQS Queue on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_sqs.jpeg)

##### 7. Delete Amazon DynamoDB Tables
1. Log in to the [Amazon DynamoDB Console](https://us-east-1.console.aws.amazon.com/dynamodbv2/home?region=us-east-1#tables).
2. Select created tables (`Students`, `Teachers`, `Grades`, `Materials`, `Documents`, `Classes`) → Click **Delete tables**.

![Delete DynamoDB Tables on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_database.jpeg)

##### 8. Delete IAM Role
1. Log in to the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles).
2. Search for `student-portal-lambda` → Select and click **Delete**.

![Delete IAM Role on AWS Console](/images/5-Workshop/5.6-Testing-cleanup/delete_iam.jpeg)

---

🎉 **Congratulations on successfully completing the AWS Serverless Student Management Portal Workshop!**
