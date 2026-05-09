# 🚀 Day 33 – AWS Lambda Function Deployment (Serverless Introduction)

## 📘 Topic

Deploying a Serverless Function using AWS Lambda (Python Runtime)

---

## 🎯 Objective

Create a simple AWS Lambda function to demonstrate:

* Serverless computing
* Rapid deployment
* Event-driven execution
* Automatic scalability

The Lambda function should return:

```id="obj1"
Welcome to KKE AWS Labs!
```

with HTTP status code:

```id="obj2"
200
```

---

## 🏗️ Architecture Overview

```id="arch33"
User/Event
    ↓
AWS Lambda
    ↓
Python Runtime
    ↓
JSON Response
```

---

## 🧱 Requirements

| Component            | Value                      |
| -------------------- | -------------------------- |
| Lambda Function Name | `datacenter-lambda`        |
| Runtime              | Python                     |
| IAM Role             | `lambda_execution_role`    |
| Response Body        | `Welcome to KKE AWS Labs!` |
| Status Code          | `200`                      |

---

# 🛠️ Implementation Steps

---

## 1️⃣ Create IAM Role

Go to:

```id="step1"
IAM → Roles → Create role
```

---

### Trusted Entity

Select:

```id="step1a"
AWS Service
```

Use case:

```id="step1b"
Lambda
```

---

### Attach Permission Policy

Attach:

```id="step1c"
AWSLambdaBasicExecutionRole
```

---

### Role Name

```id="step1d"
lambda_execution_role
```

Create role.

---

# 2️⃣ Create Lambda Function

Go to:

```id="step2"
AWS Console → Lambda → Create function
```

---

### Choose

```id="step2a"
Author from scratch
```

---

### Function Configuration

| Field         | Value               |
| ------------- | ------------------- |
| Function name | `datacenter-lambda` |
| Runtime       | Python 3.x          |
| Architecture  | x86_64              |

---

### Permissions

Select:

```id="step2b"
Use existing role
```

Choose:

```id="step2c"
lambda_execution_role
```

---

# 3️⃣ Configure Lambda Code

Replace default code with:

```python id="code1"
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

# 4️⃣ Deploy Function

Click:

```id="step4"
Deploy
```

---

# 5️⃣ Test Lambda Function

Click:

```id="step5"
Test
```

Create test event:

* Event Name: `test-event`

Save and run.

---

# ✅ Expected Output

```json id="output1"
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# 🧪 Validation Checklist

* ✔ Lambda function created
* ✔ Runtime = Python
* ✔ IAM role attached correctly
* ✔ Function deployed successfully
* ✔ Status code = 200
* ✔ Response body correct

---

# 🚨 Common Issues Faced

## 🔴 Issue 1: Wrong Handler Name

### Problem

Lambda execution fails with:

```id="err1"
Handler not found
```

### Root Cause

Incorrect function name or file name.

---

## 🔴 Issue 2: Missing IAM Permissions

### Problem

CloudWatch logs not generated.

### Root Cause

Missing:

```id="err2"
AWSLambdaBasicExecutionRole
```

---

## 🔴 Issue 3: Syntax Errors

### Problem

Function fails during execution.

### Root Cause

Improper Python indentation.

---

## 🔴 Issue 4: Forgetting to Deploy

### Problem

Code changes not reflected.

### Root Cause

Clicked Save but not:

```id="err4"
Deploy
```

---

# 🧠 Key Learnings

* Lambda is:

  * Event-driven
  * Stateless
  * Auto-scaled by AWS

* IAM roles provide:

  * Secure execution permissions
  * Access to AWS services

* Lambda removes:

  * Server management
  * OS patching
  * Infrastructure scaling effort

---

# 🔍 Real-World Use Cases

AWS Lambda is commonly used for:

* API backends
* Automation tasks
* Scheduled jobs
* S3 event processing
* CI/CD integrations
* Serverless microservices

---

# 🏁 Final Outcome

✔ Serverless Lambda deployed successfully
✔ IAM role configured correctly
✔ Function returned expected response
✔ Demonstrated AWS serverless fundamentals

---
