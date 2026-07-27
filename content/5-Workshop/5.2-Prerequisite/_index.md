---
title: "Prerequisites"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.2. </b> "
---

Before deploying the **AWS Student Management Portal** project, you need to prepare your local development environment and configure AWS access credentials on your machine.

---

### 1. Install Required Tools

#### 1.1. Node.js & npm (Runtime Environment)
Node.js is used to run backend Lambdas locally (if debugging) and compile the React frontend.
* **Requirement**: Node.js `>= 18.x`, npm `>= 9.x`.
* **Installation**:
  * Visit the [Node.js Official Website](https://nodejs.org/) and download the latest LTS version.
  * During setup on Windows, make sure to check **"Add to PATH"**.
* **Verify**: Open your Terminal or Command Prompt and run:
  ```bash
  node --version
  npm --version
  ```

#### 1.2. Python (Used for Packaging Scripts)
Python is utilized by auto-packaging scripts to zip up Lambda codebases before deployment to AWS.
* **Requirement**: Python `>= 3.8`.
* **Installation**:
  * Download the installer from [Python](https://www.python.org/downloads/).
  * **IMPORTANT**: You must check **"Add Python to PATH"** before clicking Install.
* **Verify**:
  ```bash
  python --version
  pip --version
  ```

#### 1.3. AWS CLI v2 (AWS Resource Command-Line Management)
The AWS CLI is required to interact with AWS services and create cloud resources programmatically.
* **Installation**:
  * Download and run the MSI installer on Windows: [AWS CLI V2 MSI](https://awscli.amazonaws.com/AWSCLIV2.msi).
* **Verify**:
  ```bash
  aws --version
  ```

#### 1.4. Git Bash (Recommended for Windows Users)
The database creation and deployment scripts in this project are written as Bash shell scripts (`.sh`). If you are on Windows, installing Git Bash is highly recommended.
* **Installation**: Download [Git for Windows](https://git-scm.com/download/win).
* During installation, choose **"Use Git and optional Unix tools from Windows Command Prompt"**.

---

### 2. Configure AWS Access Credentials

To allow the AWS CLI to create resources on your behalf, you must supply access keys for an IAM User with Administrator or appropriate service policies.

#### 2.1. Retrieve Access Keys from the AWS Console
1. Log in to the [AWS Management Console](https://console.aws.amazon.com/).
2. Search for the **IAM** service in the top search bar.
3. Select **Users** in the left navigation pane and click on your User.
4. Click on the **Security credentials** tab.
5. Scroll down to **Access keys** and click **Create access key**.
6. Select **Command Line Interface (CLI)**, complete the workflow, and **download the CSV file containing the Access Key ID and Secret Access Key**.

#### 2.2. Configure AWS CLI on Your Machine
Open your Terminal (or Git Bash) and run:
```bash
aws configure
```

Fill in the prompts as shown below:
```text
AWS Access Key ID [None]: [Enter your Access Key ID]
AWS Secret Access Key [None]: [Enter your Secret Access Key]
Default region name [None]: us-east-1
Default output format [None]: json
```

> [!IMPORTANT]
> This workshop defaults to the **us-east-1** (N. Virginia) region. Ensure you configure `us-east-1` so that all resources are created within the same region, avoiding cross-region integration errors.

#### 2.3. Verify Configuration
Run the following command to check if the AWS CLI can successfully make calls to your account:
```bash
aws sts get-caller-identity
```

If it returns a JSON block containing your `Account ID`, `Arn`, and `UserId`, the credentials setup is complete, and you are ready to proceed to the next step!
