# 🚀 Day 34 – Deploying AWS Lambda Using AWS CLI and ZIP Package

## 📘 Topic

AWS Lambda Deployment using Python Runtime and AWS CLI

---

## 🎯 Objective

Deploy a Python-based AWS Lambda function using:

* AWS CLI
* ZIP package deployment
* Existing IAM execution role

This task demonstrates:

* Serverless deployment automation
* Lambda packaging workflow
* CLI-driven infrastructure operations

---

# 🏗️ Architecture Overview

```text
Python Script
     ↓
ZIP Package
     ↓
AWS CLI
     ↓
AWS Lambda
     ↓
JSON Response
```

---

# 🧱 Requirements

| Component     | Value                   |
| ------------- | ----------------------- |
| Python Script | `lambda_function.py`    |
| ZIP File      | `function.zip`          |
| Lambda Name   | `datacenter-lambda-cli` |
| Runtime       | Python                  |
| IAM Role      | `lambda_execution_role` |

---

# 🛠️ Implementation Steps

---

## 1️⃣ Create Python Script

On `aws-client` host:

```bash
vi lambda_function.py
```

Add the following code:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

Save the file.

---

## 2️⃣ Verify Script

```bash
cat lambda_function.py
```

---

## 3️⃣ Create ZIP Package

```bash
zip function.zip lambda_function.py
```

Verify files:

```bash
ls
```

Expected:

```text
function.zip
lambda_function.py
```

---

## 4️⃣ Retrieve IAM Role ARN

```bash
aws iam get-role --role-name lambda_execution_role
```

Copy the ARN from output:

```text
arn:aws:iam::<ACCOUNT-ID>:role/lambda_execution_role
```

---

## 5️⃣ Create Lambda Function

```bash
aws lambda create-function \
--function-name datacenter-lambda-cli \
--runtime python3.12 \
--role arn:aws:iam::<ACCOUNT-ID>:role/lambda_execution_role \
--handler lambda_function.lambda_handler \
--zip-file fileb://function.zip
```

---

# 🧠 Command Breakdown

| Parameter         | Purpose                 |
| ----------------- | ----------------------- |
| `--function-name` | Lambda function name    |
| `--runtime`       | Python runtime          |
| `--role`          | IAM execution role      |
| `--handler`       | `<filename>.<function>` |
| `--zip-file`      | ZIP deployment package  |

---

## 6️⃣ Verify Lambda Function

```bash
aws lambda get-function \
--function-name datacenter-lambda-cli
```

---

## 7️⃣ Invoke Lambda Function

```bash
aws lambda invoke \
--function-name datacenter-lambda-cli \
output.json
```

Expected terminal output:

```json
{
    "StatusCode": 200,
    "ExecutedVersion": "$LATEST"
}
```

---

## 8️⃣ Check Actual Lambda Response

```bash
cat output.json
```

Expected:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# ✅ Validation Checklist

* ✔ Python script created
* ✔ ZIP package generated
* ✔ Lambda function deployed successfully
* ✔ Runtime configured correctly
* ✔ IAM role attached successfully
* ✔ Lambda invocation successful
* ✔ Function returns expected response

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: JSON Syntax Error

### Error

```text
JSON syntax error at index 0 line 1 column 0
```

### Root Cause

Python Lambda code was mistakenly pasted into IAM policy JSON editor.

---

### Fix

* IAM policy editor → accepts JSON only
* Lambda code editor → accepts Python code

Correct Lambda code:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

## 🔴 Issue 2: Incorrect Multi-Line Command Syntax

### Problem

Spaces added after `\`

Incorrect:

```bash
--runtime python3.12 \ 
```

---

### Fix

Correct:

```bash
--runtime python3.12 \
```

---

## 🔴 Issue 3: Wrong IAM ARN

### Problem

Account ID duplicated in ARN.

Incorrect:

```text
arn:aws:iam::605399109188605399109188:role/lambda_execution_role
```

---

### Fix

Correct format:

```text
arn:aws:iam::<ACCOUNT-ID>:role/lambda_execution_role
```

---

## 🔴 Issue 4: Confusion About `$LATEST`

### Problem

Expected:

```text
Welcome to KKE AWS Labs!
```

But terminal showed:

```json
"ExecutedVersion": "$LATEST"
```

---

### Explanation

AWS CLI returns:

* Execution metadata in terminal
* Actual Lambda response inside `output.json`

---

### Actual Function Output

```bash
cat output.json
```

Result:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

## 🔴 Issue 5: ZIP Structure Problems

### Problem

Lambda unable to locate handler.

---

### Root Cause

ZIP contained nested folders.

Correct ZIP structure:

```text
function.zip
 └── lambda_function.py
```

---

# 🧠 Key Learnings

* Lambda deployments can be automated using AWS CLI
* ZIP packaging is standard for Lambda deployments
* IAM roles provide secure execution permissions
* `$LATEST` refers to Lambda version, not function output
* Handler naming convention is critical

---

# 🔍 Real-World Use Cases

AWS Lambda is widely used for:

* Event-driven automation
* API backends
* CI/CD integrations
* Serverless microservices
* Scheduled jobs
* Infrastructure automation

---

# 🏁 Final Outcome

✔ Lambda deployed successfully using AWS CLI
✔ ZIP package deployment completed
✔ Function executed successfully
✔ Expected JSON response validated
✔ Serverless deployment workflow automated

---

# 💡 DevOps Insight

> “Manual deployments teach concepts.
> CLI deployments teach automation.”
