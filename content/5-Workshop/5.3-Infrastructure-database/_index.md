---
title: "Infrastructure & Database"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

In this section, we will initialize the core backend infrastructure for data storage, user authentication, and message queuing: **DynamoDB**, **S3 Bucket**, **SQS Queue**, and **Amazon Cognito**.

The project provides pre-configured automation scripts to streamline this setup.

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

### Step 2: Create DynamoDB Tables

The Student Management Portal uses 5 independent DynamoDB tables to store business data:
* **Students**: Stores student records (PK: `id` - studentId).
* **Teachers**: Stores teacher profiles (PK: `id` - teacherId).
* **Grades**: Stores grades for subjects (PK: `id` - `${studentId}-${subject}-${timestamp}`).
* **Materials**: Stores learning resources published by teachers (PK: `id` - `${type}-${timestamp}`).
* **Documents**: Stores student document metadata (PK: `id` - `${studentId}-${timestamp}`).

Run the deployment script to create all these tables on AWS using `PAY_PER_REQUEST` (On-Demand) billing mode:

```bash
cd ..
bash scripts/deploy-dynamodb.sh us-east-1
```

*(Alternatively, you can manually run `aws dynamodb create-table` commands for each table with their corresponding Partition Keys).*

---

### Step 3: Create S3 Document Bucket & Configure CORS

An S3 Bucket is used to store physical documents and course files uploaded by students and teachers. Since the React Frontend (running in the browser) uploads files directly to S3 via **Presigned URLs**, we must configure **CORS** (Cross-Origin Resource Sharing) to prevent the browser from blocking requests.

#### 1. Create the S3 Document Bucket
Note: S3 bucket names must be **globally unique**. Replace `<yourname>` with your initials or custom identifier:
```bash
aws s3 mb s3://student-documents-<yourname> --region us-east-1
```

#### 2. Apply CORS Configuration
Apply the `cors.json` rules defined in the project:
```bash
aws s3api put-bucket-cors --bucket student-documents-<yourname> --cors-configuration file://cors.json
```

The `cors.json` configuration permits standard HTTP methods like `PUT` and `POST` from any client source (`*`):
```json
{
  "CORSRules": [
    {
      "AllowedHeaders": ["*"],
      "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
      "AllowedOrigins": ["*"],
      "ExposeHeaders": ["ETag"]
    }
  ]
}
```

---

### Step 4: Create SQS Queue for Notifications

Create an SQS message queue to bridge synchronous application events (e.g. grade updates, document creation) to the background Lambda Email Worker:

```bash
aws sqs create-queue --queue-name student-notifications --region us-east-1
```

Copy the returned **Queue URL** (e.g. `https://sqs.us-east-1.amazonaws.com/123456789/student-notifications`) for later use.

---

### Step 5: Setup Amazon Cognito User Pool

Amazon Cognito manages user registration, login, and JWT token issuance to secure APIs exposed via API Gateway.

Run the automatic Cognito setup script:
```bash
bash scripts/setup-cognito.sh us-east-1
```

This script performs the following tasks:
1. Creates a Cognito User Pool named `student-portal-user-pool`.
2. Creates an App Client for React integration (with client secrets disabled for browser compatibility).
3. Configures 3 User Groups: `Admin`, `Teacher`, and `Student`.
4. Creates a default administrator demo account:
   * **Username**: `admin@example.com`
   * **Temporary Password**: `Abc12345!` (you will be prompted to reset it upon first sign-in).

**Important**: Make sure to save the **UserPoolId** and **AppClientId** outputs printed to the console for the next deployment steps.
