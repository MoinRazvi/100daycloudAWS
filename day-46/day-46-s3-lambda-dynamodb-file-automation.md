# 🚀 Day 46 – Automated File Transfer Between S3 Buckets Using AWS Lambda

## 📘 Topic

Automating secure file movement between S3 buckets using AWS Lambda and logging operations into DynamoDB.

---

# 🎯 Objective

The DevOps team needed to automate file handling using AWS services.

The workflow requirements were:

* Upload files into a **public S3 bucket**
* Automatically trigger a Lambda function
* Copy uploaded files into a **private S3 bucket**
* Store operation logs in DynamoDB

This task demonstrates:

* Event-driven AWS architecture
* S3 bucket automation
* Lambda triggers
* IAM roles and policies
* DynamoDB logging
* Secure file transfer workflows

---

# 🏗️ AWS Architecture Overview

```text id="g1"
User Uploads File
        ↓
Public S3 Bucket
(devops-public-26170)
        ↓
S3 Event Notification
(ObjectCreated)
        ↓
AWS Lambda Function
(devops-copyfunction)
        ↓
Copies File
        ↓
Private S3 Bucket
(devops-private-27450)
        ↓
Stores Logs
        ↓
DynamoDB Table
(devops-S3CopyLogs)
```

---

# 🧱 AWS Components Used

| Component         | Name                    |
| ----------------- | ----------------------- |
| Public S3 Bucket  | `devops-public-26170`   |
| Private S3 Bucket | `devops-private-27450`  |
| Lambda Function   | `devops-copyfunction`   |
| IAM Role          | `lambda_execution_role` |
| DynamoDB Table    | `devops-S3CopyLogs`     |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Public S3 Bucket

```bash id="gh1"
aws s3 mb s3://devops-public-26170
```

---

# 2️⃣ Configure Public Access

Disabled block public access and attached public-read bucket policy.

---

# 3️⃣ Create Private S3 Bucket

```bash id="gh2"
aws s3 mb s3://devops-private-27450
```

---

# 4️⃣ Create DynamoDB Table

```bash id="gh3"
aws dynamodb create-table \
--table-name devops-S3CopyLogs \
--attribute-definitions AttributeName=LogID,AttributeType=S \
--key-schema AttributeName=LogID,KeyType=HASH \
--billing-mode PAY_PER_REQUEST
```

---

# 5️⃣ Create IAM Role

Created IAM role:

```text id="gh4"
lambda_execution_role
```

with:

* S3 access
* DynamoDB access
* CloudWatch logging permissions

---

# 6️⃣ Configure Lambda Function

Created Lambda function:

```text id="gh5"
devops-copyfunction
```

Runtime:

```text id="gh6"
Python 3.12
```

---

# 7️⃣ Update Lambda Python Script

Modified:

* private bucket name
* DynamoDB table name

inside:

```text id="gh7"
lambda-function.py
```

---

# 8️⃣ Package Lambda Function

```bash id="gh8"
zip function.zip lambda-function.py
```

---

# 9️⃣ Upload Lambda Function

```bash id="gh9"
aws lambda update-function-code \
--function-name devops-copyfunction \
--zip-file fileb://function.zip
```

---

# 🔟 Configure S3 Trigger

Configured:

```text id="gh10"
ObjectCreated
```

event notification on:

```text id="gh11"
devops-public-26170
```

to invoke:

```text id="gh12"
devops-copyfunction
```

---

# 1️⃣1️⃣ Upload Test File

```bash id="gh13"
aws s3 cp /root/sample.zip s3://devops-public-26170/
```

---

# 1️⃣2️⃣ Verify File Copy

```bash id="gh14"
aws s3 ls s3://devops-private-27450/
```

Expected:

```text id="gh15"
sample.zip
```

---

# 1️⃣3️⃣ Verify DynamoDB Logs

```bash id="gh16"
aws dynamodb scan --table-name devops-S3CopyLogs
```

Verified:

* Source bucket
* Destination bucket
* Object key
* Timestamp
* Status

---

# ✅ Final Validation

* ✔ Public bucket configured
* ✔ Private bucket configured
* ✔ Lambda trigger working
* ✔ File copied successfully
* ✔ DynamoDB logs created
* ✔ Event-driven automation verified

---

# 🚨 Real Troubleshooting Issues Faced

---

# 🔴 Issue 1: File Uploaded to Public Bucket But Not Copied

## Symptoms

* Upload succeeded in public bucket
* Private bucket remained empty
* No DynamoDB logs created

---

## Root Cause

Lambda trigger was attached to:

```text id="gh17"
devops-private-27450
```

instead of:

```text id="gh18"
devops-public-26170
```

---

## Fix

Configured S3 event notification correctly on:

```text id="gh19"
devops-public-26170
```

for:

```text id="gh20"
ObjectCreated
```

events.

---

# 🔴 Issue 2: CloudWatch Log Group Missing

## Error

```text id="gh21"
Log group '/aws/lambda/devops-copyfunction' does not exist
```

---

## Root Cause

Lambda never executed because trigger was incorrect.

---

## Fix

Corrected S3 trigger configuration and re-uploaded test file.

CloudWatch logs were created automatically after first successful execution.

---

# 🔴 Issue 3: Lambda Code Changes Not Reflecting

## Root Cause

Lambda zip package updated locally but not uploaded.

---

## Fix

Recreated zip:

```bash id="gh22"
zip function.zip lambda-function.py
```

Updated Lambda:

```bash id="gh23"
aws lambda update-function-code \
--function-name devops-copyfunction \
--zip-file fileb://function.zip
```

---

# 🔴 Issue 4: Event Notification Not Configured

## Root Cause

S3 bucket notification configuration missing.

---

## Fix

Configured:

```text id="gh24"
s3:ObjectCreated:*
```

event notifications.

---

# 🔴 Issue 5: Incorrect Resource Names in Lambda Code

## Root Cause

Placeholder values remained unchanged.

---

## Fix

Updated:

```python id="gh25"
REPLACE-WITH-YOUR-PRIVATE-BUCKET
```

with:

```python id="gh26"
devops-private-27450
```

and:

```python id="gh27"
REPLACE-WITH-YOUR-DYNAMODB-TABLE
```

with:

```python id="gh28"
devops-S3CopyLogs
```

---

# 🔴 Issue 6: IAM Role Permissions Verification

## Required Permissions

Lambda role required:

* s3:GetObject
* s3:PutObject
* dynamodb:PutItem
* CloudWatch logging permissions

---

# 🧠 Key Learnings

* S3 events trigger serverless workflows
* Lambda automates object processing
* Event source configuration is critical
* CloudWatch logs are essential for debugging
* IAM permissions directly impact Lambda execution
* DynamoDB provides lightweight operational logging

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* File processing pipelines
* Media transcoding
* Secure document ingestion
* Automated backup workflows
* Compliance logging systems
* Serverless event-driven applications

---

# 🏁 Final Outcome

✔ Public-to-private S3 automation implemented
✔ Lambda trigger configured successfully
✔ File copy validated successfully
✔ DynamoDB logging working correctly
✔ Event-driven AWS workflow completed

---

# 💡 DevOps Insight

> “In serverless architectures, the event source configuration is just as important as the application code itself.”
#
<img width="1637" height="961" alt="image" src="https://github.com/user-attachments/assets/cf156479-3616-459a-a8ce-ce1695ef0da9" />

