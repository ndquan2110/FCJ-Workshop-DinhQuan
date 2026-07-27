---
title: "Frontend Deployment & Hosting"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.5. </b> "
---

In this section, we will review the frontend structure of the **AWS Student Management Portal**, configure environment variables for AWS Cognito and API Gateway, test the application locally, bundle production assets onto **Amazon S3**, and distribute global static web assets via **Amazon CloudFront**.

---

### Step 1: Frontend Structure & Authorization

The Frontend application is built with **ReactJS** and bundled using **Vite**.

![AWS Student Management Portal Web UI](/images/5-Workshop/5.5-Frontend-deployment/student_portal_app_ui.png)

* **Protected Routing**: The application uses a `ProtectedRoute` component to validate the user's Cognito-issued JWT Token.
* **Group-Based Authorization**:
  * **Admin/Staff**: Access to all modules including Dashboard, Student/Teacher/Grades management, System Logs, and Admin Settings.
  * **Teacher**: Access to assigned class rosters, posting/updating student grades, and uploading learning materials.
  * **Student**: Access to personal profile, viewing personal grades, and downloading learning materials uploaded by teachers.

---

### Step 2: Configure Frontend Environment Variables (.env)

To connect the React frontend with your AWS backend infrastructure, create a `frontend/.env` file inside the `frontend/` directory with your actual resource parameters:

```env
# AWS Cognito User Pool Configuration
VITE_USER_POOL_ID=us-east-1_7SwNQ0qYm
VITE_APP_CLIENT_ID=student-portal-react-client

# API Gateway Endpoint (Actual Invoke URL)
VITE_API_ENDPOINT=https://9k9i3ukwdh.execute-api.us-east-1.amazonaws.com/prod

# AWS Region
VITE_AWS_REGION=us-east-1
```

---

### Step 3: Local Testing & Verification

With `.env` configured, start the local development server to test login authentication and dashboard routing:

```bash
cd frontend
npm run dev
```

Open your browser and navigate to [http://localhost:5173](http://localhost:5173). 

Test sign-in using the default admin demo credentials:
* **Email**: `admin@example.com`
* **Password**: `Abc12345!`

Upon first login, Cognito will prompt you to set a permanent new password.

![Student Portal Admin Page UI](/images/5-Workshop/5.5-Frontend-deployment/student_portal_admin.jpeg)

---

### Step 4: Create S3 Frontend Bucket & Upload Build Assets via AWS Web Console

Once the local application runs cleanly, bundle the React application into production static HTML/JS/CSS assets to host on Amazon S3.

#### 📌 Step-by-Step AWS Web Console Guide:
1. Run the build command locally:
   ```bash
   cd frontend
   npm run build
   ```
   This creates a `dist/` directory containing bundled static assets.

2. Log in to the [Amazon S3 Console](https://us-east-1.console.aws.amazon.com/s3/buckets?region=us-east-1).
3. Click **Create bucket**.

![Create S3 Frontend Bucket Console UI](/images/5-Workshop/5.5-Frontend-deployment/create_bucket_s3.png)

4. Enter **Bucket name**: `student-portal-frontend-147997148454` *(Bucket name must be globally unique)*.
5. Select **AWS Region**: `us-east-1` (US East - N. Virginia).
6. Under **Block Public Access settings**, keep **Block *all* public access** checked to protect the S3 bucket (allowing access solely via CloudFront OAC).
7. Scroll down and click **Create bucket**.
8. Click on your created bucket `student-portal-frontend-147997148454` → Click **Upload**.
9. Drag and drop all files and folders from inside `dist/` into the upload window and click **Upload**.

![Upload Static Assets to S3 Bucket Console UI](/images/5-Workshop/5.5-Frontend-deployment/Upload_bucket.png)

---

### Step 5: Initialize & Distribute via Amazon CloudFront via AWS Web Console

To optimize global load performance and enable HTTPS security, deploy an **Amazon CloudFront Distribution** pointing to your frontend S3 bucket as its Origin.

#### 📌 Step-by-Step AWS Web Console Guide:
1. Log in to the [Amazon CloudFront Console](https://us-east-1.console.aws.amazon.com/cloudfront/v3/home?region=us-east-1#/distributions).
2. Click **Create distribution**.
3. Under **Origin domain**, select your frontend S3 bucket (`student-portal-frontend-147997148454.s3.amazonaws.com`).
4. Under **Origin access**, select **Origin access control settings (recommended)** (OAC) → Click **Create control setting** → Keep defaults and click **Create**.
5. Under **Default cache behavior**:
   - For **Viewer protocol policy**, select **Redirect HTTP to HTTPS**.
   - For **Allowed HTTP methods**, select **GET, HEAD**.
6. Under **Settings**:
   - Enter **Default root object**: `index.html`.
7. Click **Create distribution**.
8. Copy the generated **S3 bucket policy** and paste it into the **Permissions** > **Bucket policy** section of your frontend S3 bucket to grant read access to CloudFront.

![Update S3 Bucket Policy for CloudFront OAC](/images/5-Workshop/5.5-Frontend-deployment/Upload_bucket.png)

9. Wait for the distribution status to change to **Enabled** (approx. 3-5 minutes). Retrieve your live CloudFront domain:
   ```text
   https://d3th0yl82lu593.cloudfront.net/
   ```

![Amazon CloudFront Distribution E39TFB7INWHA6Y Details](/images/5-Workshop/5.5-Frontend-deployment/CloudFront_Distributions_E39TFB7INWHA6Y.png)

---

### Step 6: Demo Test Accounts & Role Authorization Table

| Role Group | Login Email | Password | Authorized Features |
|------------|-------------|----------|---------------------|
| **Admin** | `admin@example.com` | `Abc12345!` | Full system access (Teacher management, logs, accounts). |
| **Teacher / Staff** | `staff@example.com` | `Abc12345!` | Access Students, Grades, Learning Materials, Profiles, Notifications. |
| **Student** | `student@example.com` | `Abc12345!` | Access Overview, Personal Profile, Grade View, Learning Materials, and Notifications. |

![Student Portal Admin Page UI](/images/5-Workshop/5.5-Frontend-deployment/student_portal_admin.jpeg)

![Student Portal Staff Page UI](/images/5-Workshop/5.5-Frontend-deployment/student_portal_staff.jpeg)

![Student Portal Student Page UI](/images/5-Workshop/5.5-Frontend-deployment/student_portal_student.jpeg)
