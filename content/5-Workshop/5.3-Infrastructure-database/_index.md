---
title: "Infrastructure & Database"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 5.3. </b> "
---

In this section, we will manually create and configure the core backend infrastructure services on the **AWS Management Console**: **DynamoDB**, **S3 Bucket**, **SQS Queue**, and **Amazon Cognito**.

---

### Step 1: Install Dependencies (npm packages)

Navigate to the project root on your machine and install required libraries for both backend and frontend:

```bash
# 1. Install backend dependencies
cd backend
npm install

# 2. Install frontend dependencies
cd ../frontend
npm install
```

---

### Step 2: Create Amazon DynamoDB Tables via AWS Web Console

The Student Management Portal uses 6 independent DynamoDB tables to store business data:

* **Students**: Stores student profiles (Partition key: `id` - String).
* **Teachers**: Stores teacher profiles (Partition key: `id` - String).
* **Classes**: Stores course rosters (Partition key: `id` - String).
* **Grades**: Stores grade records (Partition key: `id` - String).
* **Materials**: Stores learning resources (Partition key: `id` - String).
* **Documents**: Stores student document metadata (Partition key: `id` - String).

#### 📌 Step-by-Step AWS Web Console Guide:
1. Log in to the [AWS DynamoDB Console](https://us-east-1.console.aws.amazon.com/dynamodbv2/home?region=us-east-1#tables).
2. In the left navigation pane, select **Tables** → Click **Create table**.
3. Enter **Table name**: `Students`.
4. Enter **Partition key**: `id`, select data type **String**.
5. Under **Table settings**, select **Customize settings** → Under **Read/write capacity settings**, select **On-demand** (PAY_PER_REQUEST).
6. Scroll down and click **Create table**.
7. Repeat steps 2 - 6 for the remaining 5 tables: `Teachers`, `Classes`, `Grades`, `Materials`, and `Documents`.
8. Verify all 6 tables display with an **Active** status.

![Create DynamoDB Table on AWS Console UI](/images/5-Workshop/5.3-Infrastructure-database/aws_console_dynamodb_create.png)

![Amazon DynamoDB Tables AWS Console UI](/images/5-Workshop/5.3-Infrastructure-database/dynamodb_tables_console.png)

---

### Step 3: Create S3 Document Bucket & Configure CORS via AWS Web Console

An S3 Bucket is used to store physical documents and course files uploaded by students and teachers. Since the React Frontend (running in the browser) uploads files directly to S3 via **Presigned URLs**, we must configure **CORS** (Cross-Origin Resource Sharing) to prevent the browser from blocking requests.

#### 📌 Step-by-Step AWS Web Console Guide:

##### 1. Create S3 Document Bucket
1. Log in to the [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
2. Click **Create bucket**.
3. Enter **Bucket name**: `student-documents-147997148454` *(Bucket names must be globally unique)*.
4. Select **AWS Region**: `us-east-1` (US East - N. Virginia).
5. Under **Block Public Access settings for this bucket**, keep **Block *all* public access** checked.
6. Scroll down and click **Create bucket**.

##### 2. Configure CORS Rules for S3 Bucket
1. From the Buckets list, click on your created bucket (`student-documents-147997148454`).
2. Select the **Permissions** tab.
3. Scroll down to **Cross-origin resource sharing (CORS)** and click **Edit**.
4. Paste the following JSON configuration:

```json
[
  {
    "AllowedHeaders": [
      "*"
    ],
    "AllowedMethods": [
      "GET",
      "PUT",
      "POST",
      "DELETE",
      "HEAD"
    ],
    "AllowedOrigins": [
      "*"
    ],
    "ExposeHeaders": [
      "ETag"
    ]
  }
]
```

5. Click **Save changes**.

![Configure S3 Bucket & CORS on AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_console_s3_create.png)

---

### Step 4: Create SQS Queue for Notifications via AWS Web Console

Create an SQS message queue to bridge synchronous application events to the background Lambda Email Worker.

#### 📌 Step-by-Step AWS Web Console Guide:
1. Log in to the [Amazon SQS Console](https://us-east-1.console.aws.amazon.com/sqs/v2/home?region=us-east-1#/queues).
2. Click **Create queue**.
3. Select type: **Standard**.
4. Enter **Name**: `student-notifications`.
5. Keep default timeout settings (Visibility timeout: `30 seconds`, Message retention period: `4 days`).
6. Scroll down and click **Create queue**.
7. Copy and save the **Queue URL**:
   ```text
   https://sqs.us-east-1.amazonaws.com/147997148454/student-notifications
   ```

![Create Amazon SQS Queue on AWS Web Console](/images/5-Workshop/5.3-Infrastructure-database/aws_sqs_create.png)

---

### Step 5: Setup Amazon Cognito User Pool via AWS Web Console

Amazon Cognito manages user registration, login, and JWT token issuance to secure APIs exposed via API Gateway.

#### 📌 Step-by-Step AWS Web Console Guide:

##### 1. Create Cognito User Pool
1. Log in to the [Amazon Cognito Console](https://us-east-1.console.aws.amazon.com/cognito/v2/idp/user-pools?region=us-east-1).
2. Click **Create user pool**.
3. **Step 1**: Select **Email** as provider attribute. Click **Next**.
4. **Step 2**: Select **No MFA**. Click **Next**.
5. **Step 3**: Keep default settings. Click **Next**.
6. **Step 4**: Select **Send email with Cognito**. Click **Next**.
7. **Step 5**:
   * Enter **User pool name**: `student-portal-user-pool` *(User Pool ID: `us-east-1_7SwNQ0qYm`)*.
   * Under **App client**, select **Public client**.
   * Enter **App client name**: `student-portal-react-client`.
   * Ensure **Don't generate a client secret** is checked.
8. **Step 6**: Review settings and click **Create user pool**.

##### 2. Create User Groups
1. Click on your created User Pool (`student-portal-user-pool`).
2. Select the **Groups** tab → Click **Create group**.
3. Create 3 groups:
   * Group 1: Name = `ADMIN`, Description = `System Administrators`.
   * Group 2: Name = `TEACHER`, Description = `Teachers / Staff`.
   * Group 3: Name = `STUDENT`, Description = `Students`.

##### 3. Create Admin Demo Account
1. Select the **Users** tab → Click **Create user**.
2. Select **Send an email invitation**.
3. Enter **Email address**: `admin@example.com`.
4. Enter **Temporary password**: `Abc12345!`.
5. Click **Create user**, then assign this user to the `ADMIN` group.

![Amazon Cognito User Pool AWS Console UI](/images/5-Workshop/5.3-Infrastructure-database/aws_console_cognito_create.png)

![Amazon Cognito User Pool Details AWS Console UI](/images/5-Workshop/5.3-Infrastructure-database/cognito_user_pool_console.png)
