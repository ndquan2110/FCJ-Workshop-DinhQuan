---
title: "Backend & API Gateway Deployment"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.4. </b> "
---

In this section, we will manually build and configure the entire Backend infrastructure on the **AWS Management Console**, including creating IAM Roles, setting up **AWS Lambda** functions (Node.js), and configuring **Amazon API Gateway** with **Cognito Authorizer** integration.

---

### Step 1: Create IAM Role for Lambda via AWS Web Console

Before deploying Lambda functions, we need to create an IAM Role to grant least-privilege permissions required by Lambda to interact with DynamoDB, S3, SQS, SES, and CloudWatch Logs.

#### 📌 Step-by-Step AWS Web Console Guide:
1. Log in to the [AWS IAM Console](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/roles).
2. In the left navigation pane, click **Roles** → Click **Create role**.
3. **Step 1 (Select trusted entity)**: Select **AWS service**, under Use case select **Lambda**. Click **Next**.
4. **Step 2 (Add permissions)**: Search for and attach the following policies:
   - `AWSLambdaBasicExecutionRole` (Permission to write logs to CloudWatch)
   - `AmazonDynamoDBFullAccess` (Permission to read/write 6 DynamoDB tables)
   - `AmazonS3FullAccess` (Permission to read/write S3 Buckets & create Presigned URLs)
   - `AmazonSQSFullAccess` (Permission to send/receive messages on SQS Queue)
   - `AmazonSESFullAccess` (Permission to send notification emails via SES)
5. Click **Next**.
6. **Step 3 (Name, review, and create)**:
   - Enter **Role name**: `student-portal-lambda`.
   - Review permissions and click **Create role**.
7. Copy and save the created **Role ARN**:
   ```text
   arn:aws:iam::147997148454:role/student-portal-lambda
   ```

![AWS IAM Roles List Console UI](/images/5-Workshop/5.4-Backend-apigateway/iam_roles.png)

![AWS IAM Users List Console UI](/images/5-Workshop/5.4-Backend-apigateway/iam_users.png)

---

### Step 2: Create & Configure AWS Lambda Functions via AWS Web Console

The Student Management Portal utilizes 33 Lambda functions handling separate business workflows (CRUD operations for students, teachers, classes, grades, material uploads, and background email workers).

#### 📌 Step-by-Step AWS Web Console Guide:
1. Log in to the [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions).
2. Click **Create function**.
3. Select **Author from scratch**.
4. Enter **Function name**: `getStudents` *(Repeat for remaining functions like `createStudent`, `docUploadUrl`, `sendEmailWorker`, etc.)*.
5. Select **Runtime**: `Node.js 18.x` (or `Node.js 20.x`).
6. Under **Change default execution role**:
   - Select **Use an existing role**.
   - Under **Existing role**, select `student-portal-lambda`.
7. Click **Create function**.
8. **Configure Environment Variables**:
   - Navigate to the **Configuration** tab → Select **Environment variables** in the left menu.
   - Click **Edit** → Add the following Key/Value environment variables:
     - `DOCUMENTS_BUCKET`: `student-documents-147997148454`
     - `NOTIFICATION_QUEUE_URL`: `https://sqs.us-east-1.amazonaws.com/147997148454/student-notifications`
     - `USER_POOL_ID`: `us-east-1_7SwNQ0qYm`
   - Click **Save**.
9. Under the **Code** tab, upload your function's `.zip` source code bundle and click **Deploy**.

![Create AWS Lambda Function on AWS Console UI](/images/5-Workshop/5.4-Backend-apigateway/aws_console_lambda_create.png)

![AWS Lambda Functions List Console UI](/images/5-Workshop/5.4-Backend-apigateway/lambda.jpeg)

---

### Step 3: Deploy & Configure REST API Gateway via AWS Web Console

To allow the Frontend application to interact securely with Lambda functions, we set up **Amazon API Gateway** as an API routing gateway integrated with **Cognito Authorizer**.

#### 📌 Step-by-Step AWS Web Console Guide:

##### 1. Initialize REST API
1. Log in to the [Amazon API Gateway Console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
2. Click **Create API** → Under **REST API**, click **Build**.
3. Select **New API**.
4. Enter **API name**: `student-portal-api` *(REST API ID: `9k9i3ukwdh`)*.
5. Select **Endpoint Type**: `Regional`. Click **Create API**.

![Initialize REST API on AWS Console UI](/images/5-Workshop/5.4-Backend-apigateway/create_rest_api.jpeg)

##### 2. Create Security Cognito Authorizer
1. In the left menu of `student-portal-api`, select **Authorizers**.
2. Click **Create new authorizer**.
3. Enter **Authorizer name**: `CognitoAuthorizer`.
4. Select **Type**: `Cognito`.
5. Under **Cognito User Pool**, select `student-portal-user-pool` (`us-east-1_7SwNQ0qYm`).
6. Enter **Token Source**: `Authorization`. Click **Create authorizer**.

![Create Security Cognito Authorizer](/images/5-Workshop/5.4-Backend-apigateway/create_authorizer.jpeg)

##### 3. Create Resources & HTTP Methods
1. In the left menu, click **Resources**.
2. Click **Actions** → Select **Create Resource**:
   - Enter Resource Name: `students` → Resource Path: `/students`. Click **Create Resource**.
3. Select the created `/students` resource → Click **Actions** → Select **Create Method**:
   - Create **GET** method: Select Integration type `Lambda Function` → Select Lambda function `getStudents` → Click **Save**.
   - Create **POST** method: Select Integration type `Lambda Function` → Select Lambda function `createStudent` → Click **Save**.
4. Repeat for other Resources: `/teachers`, `/grades`, `/documents`, `/materials`.
5. **Enable Cognito Security**: Select a method (e.g. POST `/students`) → Click **Method Request** → Under **Authorization**, change `NONE` to `CognitoAuthorizer` → Click the checkmark to save.

![Create Resources & HTTP Methods on AWS Console UI](/images/5-Workshop/5.4-Backend-apigateway/create_resource.jpeg)

##### 4. Enable CORS & Deploy API Stage `prod`
1. Select root resource `/` or child resources → Click **Actions** → Select **Enable CORS**.
2. Keep default settings and click **Enable CORS and replace existing CORS headers**.
3. Click **Actions** → Select **Deploy API**.
4. Under **Deployment stage**, select **[New Stage]** → Enter **Stage name**: `prod`. Click **Deploy**.
5. Copy the **Invoke URL** displayed on the Stage Overview page:
   ```text
   https://9k9i3ukwdh.execute-api.us-east-1.amazonaws.com/prod
   ```

![Amazon API Gateway & Lambda AWS Console UI](/images/5-Workshop/5.4-Backend-apigateway/apigateway_lambda_console.png)
