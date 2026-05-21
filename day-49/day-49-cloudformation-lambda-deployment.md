# � Day 49 – AWS Lambda Deployment Using CloudFormation

## 📘 Topic

Deploying an AWS Lambda function and IAM role using AWS CloudFormation Infrastructure as Code (IaC).

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create a CloudFormation template
* Deploy a Lambda function
* Create an IAM role
* Configure Lambda runtime
* Return:

  ```text
  Welcome to KKE AWS Labs!
  ```
* Ensure:

  ```text
  statusCode = 200
  ```

This task demonstrates:

* AWS CloudFormation
* Infrastructure as Code
* Lambda automation
* IAM role provisioning
* Serverless deployments

---

# 🏗️ AWS Architecture Overview

```text id="d49a1"
                CloudFormation Stack
             datacenter-lambda-app
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
 IAM Role Created             Lambda Function Created
lambda_execution_role           datacenter-lambda
         │                               │
         └───────────────┬───────────────┘
                         ▼
                Python Runtime
                         │
                         ▼
         Returns:
         {
           "statusCode": 200,
           "body": "Welcome to KKE AWS Labs!"
         }
```
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/abe837c4-2ab6-41a8-a4e0-6262c8738fa6" />

---

# 🧱 Resources Required

| Resource                | Name                          |
| ----------------------- | ----------------------------- |
| CloudFormation Template | `/root/datacenter-lambda.yml` |
| Stack Name              | `datacenter-lambda-app`       |
| Lambda Function         | `datacenter-lambda`           |
| IAM Role                | `lambda_execution_role`       |
| Runtime                 | Python                        |

---

# 🛠️ Step-by-Step Implementation

---

# 1️⃣ Login to AWS Client Host

```bash id="d49c1"
cd /root
```

---

# 2️⃣ Create CloudFormation Template

```bash id="d49c2"
vi /root/datacenter-lambda.yml
```

---

# 3️⃣ Paste CloudFormation YAML

```yaml id="d49c3"
AWSTemplateFormatVersion: '2010-09-09'
Description: Lambda Function using CloudFormation

Resources:

  LambdaExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service:
                - lambda.amazonaws.com
            Action:
              - sts:AssumeRole

      Policies:
        - PolicyName: lambda-logs-policy
          PolicyDocument:
            Version: '2012-10-17'
            Statement:

              - Effect: Allow
                Action:
                  - logs:CreateLogGroup
                  - logs:CreateLogStream
                  - logs:PutLogEvents
                Resource: "*"

  DatacenterLambda:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: datacenter-lambda
      Runtime: python3.12
      Handler: index.lambda_handler
      Timeout: 10
      Role: !GetAtt LambdaExecutionRole.Arn

      Code:
        ZipFile: |
          import json

          def lambda_handler(event, context):

              return {
                  'statusCode': 200,
                  'body': 'Welcome to KKE AWS Labs!'
              }
```

Save and exit.

---

# 4️⃣ Deploy CloudFormation Stack

```bash id="d49c4"
aws cloudformation create-stack \
--stack-name datacenter-lambda-app \
--template-body file:///root/datacenter-lambda.yml \
--capabilities CAPABILITY_NAMED_IAM
```

---

# 5️⃣ Verify Stack Creation

```bash id="d49c5"
aws cloudformation describe-stacks \
--stack-name datacenter-lambda-app
```

Wait until:

```text id="d49c6"
CREATE_COMPLETE
```

---

# 6️⃣ Verify Lambda Function

```bash id="d49c7"
aws lambda list-functions
```

Verify:

```text id="d49c8"
datacenter-lambda
```

exists.

---

# 7️⃣ Test Lambda Function

```bash id="d49c9"
aws lambda invoke \
--function-name datacenter-lambda \
output.json
```

Expected:

```json id="d49c10"
{
  "StatusCode": 200
}
```

---

# 8️⃣ Verify Output

```bash id="d49c11"
cat output.json
```

Expected:

```json id="d49c12"
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

---

# ✅ Final Validation Checklist

* ✔ CloudFormation template created
* ✔ IAM role created
* ✔ Lambda function created
* ✔ Python runtime configured
* ✔ Stack deployed successfully
* ✔ Lambda invocation successful
* ✔ Status code = 200
* ✔ Correct response body returned

---

# 🚨 Real Troubleshooting Issues Faced

---

# 🔴 Issue 1: Stack Creation Failed

## Root Cause

Forgot IAM capability flag.

---

## Fix

Use:

```bash id="d49c13"
--capabilities CAPABILITY_NAMED_IAM
```

---

# 🔴 Issue 2: Lambda Function Creation Failed

## Root Cause

Incorrect indentation in YAML.

---

## Fix

Ensure proper YAML formatting.

---

# 🔴 Issue 3: Invalid Runtime Error

## Root Cause

Wrong runtime value.

---

## Fix

Use:

```text id="d49c14"
python3.12
```

---

# 🔴 Issue 4: Lambda Handler Error

## Error

```text id="d49c15"
Handler 'index.lambda_handler' missing
```

---

## Fix

Ensure:

```text id="d49c16"
Handler: index.lambda_handler
```

matches inline code.

---

# 🔴 Issue 5: AccessDeniedException

## Root Cause

IAM role missing CloudWatch permissions.

---

## Fix

Add:

* logs:CreateLogGroup
* logs:CreateLogStream
* logs:PutLogEvents

---

# 🔴 Issue 6: Stack Remains CREATE_IN_PROGRESS

## Root Cause

IAM propagation delay.

---

## Fix

Wait 1–2 minutes and recheck stack status.

---

# 🧠 Key Learnings

* CloudFormation automates AWS infrastructure deployment
* Lambda supports inline Python code
* IAM roles are mandatory for Lambda execution
* Infrastructure as Code improves repeatability
* YAML indentation is critical in CloudFormation

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Serverless APIs
* Automated infrastructure provisioning
* Event-driven applications
* Cloud-native automation
* CI/CD deployments

---

# 🏁 Final Outcome

✔ Lambda function deployed successfully
✔ CloudFormation stack created
✔ IAM role attached correctly
✔ Lambda returned expected response
✔ Serverless application deployment automated

---

# 💡 DevOps Insight

> “Infrastructure as Code transforms cloud deployments from manual operations into reliable, version-controlled automation.”
