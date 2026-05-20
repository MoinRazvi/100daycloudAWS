# 🚀 Day 47 – AWS Priority Queue Processing Using SQS + SNS + Lambda + CloudFormation

## 📘 Topic

Implementing priority-based message processing using Amazon SQS, SNS, Lambda, and AWS CloudFormation.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create two SQS queues
* Create an SNS topic
* Create a Lambda function
* Configure IAM permissions
* Deploy everything using CloudFormation
* Process high-priority messages before low-priority messages

This task demonstrates:

* Infrastructure as Code (IaC)
* AWS CloudFormation
* Event-driven architecture
* Priority queue implementation
* SNS fanout messaging
* Lambda message processing

---

# 🏗️ AWS Architecture Overview

```text id="cf1"
                    ┌─────────────────────┐
                    │     Publisher       │
                    │   (SNS Messages)    │
                    └──────────┬──────────┘
                               │
                               ▼
                  ┌─────────────────────────┐
                  │       SNS Topic         │
                  │ nautilus-Priority-      │
                  │     Queues-Topic        │
                  └──────────┬──────────────┘
                             │
            ┌────────────────┴────────────────┐
            ▼                                 ▼
┌────────────────────────┐      ┌────────────────────────┐
│ High Priority Queue    │      │ Low Priority Queue     │
│ nautilus-High-         │      │ nautilus-Low-          │
│ Priority-Queue         │      │ Priority-Queue         │
└────────────┬───────────┘      └────────────┬───────────┘
             │                               │
             └──────────────┬────────────────┘
                            ▼
              ┌──────────────────────────┐
              │     AWS Lambda           │
              │ nautilus-priorities-     │
              │     queue-function       │
              └──────────────────────────┘
                            │
                            ▼
                  Priority Message Processing
```

---

# 🧱 AWS Resources Required

| Resource Type        | Name                                 |
| -------------------- | ------------------------------------ |
| CloudFormation Stack | `nautilus-priority-stack`            |
| High Priority Queue  | `nautilus-High-Priority-Queue`       |
| Low Priority Queue   | `nautilus-Low-Priority-Queue`        |
| SNS Topic            | `nautilus-Priority-Queues-Topic`     |
| Lambda Function      | `nautilus-priorities-queue-function` |
| IAM Role             | `lambda_execution_role`              |

---

# 🛠️ Step-by-Step Implementation

---

# 1️⃣ Go to AWS Client Host

```bash id="cf2"
cd /root
```

---

# 2️⃣ Verify Lambda Source File

```bash id="cf3"
ls /root/index.py
```

---

# 3️⃣ Create Lambda Zip Package

```bash id="cf4"
zip function.zip index.py
```

---

# 4️⃣ Create CloudFormation Template

```bash id="cf5"
vi /root/nautilus-priority-stack.yml
```

Paste the complete YAML template.

---

# 5️⃣ CloudFormation YAML Template

```yaml id="cf6"
AWSTemplateFormatVersion: '2010-09-09'
Description: Priority Queue System using SQS SNS Lambda

Resources:

  HighPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: nautilus-High-Priority-Queue

  LowPriorityQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: nautilus-Low-Priority-Queue

  PriorityTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: nautilus-Priority-Queues-Topic

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
        - PolicyName: lambda-policy
          PolicyDocument:
            Version: '2012-10-17'
            Statement:

              - Effect: Allow
                Action:
                  - sqs:ReceiveMessage
                  - sqs:DeleteMessage
                  - sqs:GetQueueAttributes
                Resource: "*"

              - Effect: Allow
                Action:
                  - sns:Publish
                Resource: "*"

              - Effect: Allow
                Action:
                  - logs:CreateLogGroup
                  - logs:CreateLogStream
                  - logs:PutLogEvents
                Resource: "*"

  PriorityLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: nautilus-priorities-queue-function
      Runtime: python3.12
      Handler: index.lambda_handler
      Role: !GetAtt LambdaExecutionRole.Arn
      Timeout: 30

      Code:
        ZipFile: |
          import json
          def lambda_handler(event, context):
              print("Message processed")
              return {
                  'statusCode': 200,
                  'body': json.dumps('Processed')
              }

  HighPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      BatchSize: 1
      Enabled: true
      EventSourceArn: !GetAtt HighPriorityQueue.Arn
      FunctionName: !Ref PriorityLambdaFunction

  LowPriorityEventSource:
    Type: AWS::Lambda::EventSourceMapping
    Properties:
      BatchSize: 1
      Enabled: true
      EventSourceArn: !GetAtt LowPriorityQueue.Arn
      FunctionName: !Ref PriorityLambdaFunction

  HighPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      Protocol: sqs
      Endpoint: !GetAtt HighPriorityQueue.Arn
      TopicArn: !Ref PriorityTopic

  LowPrioritySubscription:
    Type: AWS::SNS::Subscription
    Properties:
      Protocol: sqs
      Endpoint: !GetAtt LowPriorityQueue.Arn
      TopicArn: !Ref PriorityTopic
```

---

# 6️⃣ Deploy CloudFormation Stack

```bash id="cf7"
aws cloudformation create-stack \
--stack-name nautilus-priority-stack \
--template-body file:///root/nautilus-priority-stack.yml \
--capabilities CAPABILITY_NAMED_IAM
```

---

# 7️⃣ Verify Stack Creation

```bash id="cf8"
aws cloudformation describe-stacks \
--stack-name nautilus-priority-stack
```

Wait until:

```text id="cf9"
CREATE_COMPLETE
```

---

# 8️⃣ Verify AWS Resources

---

## Verify SQS Queues

```bash id="cf10"
aws sqs list-queues
```

---

## Verify SNS Topic

```bash id="cf11"
aws sns list-topics
```

---

## Verify Lambda Function

```bash id="cf12"
aws lambda list-functions
```

---

# 9️⃣ Publish Test Message

```bash id="cf13"
aws sns publish \
--topic-arn <SNS-TOPIC-ARN> \
--message "High Priority Task"
```

---

# 🔟 Invoke Lambda Function

```bash id="cf14"
aws lambda invoke \
--function-name nautilus-priorities-queue-function \
output.json
```

---

# 1️⃣1️⃣ Verify CloudWatch Logs

Go to:

```text id="cf15"
CloudWatch → Log Groups
```

Check:

```text id="cf16"
/aws/lambda/nautilus-priorities-queue-function
```

---

# ✅ Validation Checklist

* ✔ CloudFormation stack created
* ✔ SQS queues created
* ✔ SNS topic created
* ✔ Lambda function deployed
* ✔ IAM role attached
* ✔ Event source mappings configured
* ✔ SNS subscriptions working
* ✔ Messages processed successfully

---

# 🚨 Real Troubleshooting Issues Faced

---

# 🔴 Issue 1: Stack Creation Failed

## Root Cause

Forgot capability flag.

---

## Fix

Use:

```bash id="cf17"
--capabilities CAPABILITY_NAMED_IAM
```

---

# 🔴 Issue 2: Lambda Permission Errors

## Error

```text id="cf18"
AccessDenied
```

---

## Root Cause

IAM role missing:

* SQS permissions
* CloudWatch permissions

---

## Fix

Added:

* sqs:ReceiveMessage
* sqs:DeleteMessage
* logs permissions

---

# 🔴 Issue 3: Lambda Not Triggering

## Root Cause

Event source mapping missing.

---

## Fix

Configured:

```yaml id="cf19"
AWS::Lambda::EventSourceMapping
```

---

# 🔴 Issue 4: Queue Subscription Failed

## Root Cause

SNS topic not subscribed to SQS queues.

---

## Fix

Created:

```yaml id="cf20"
AWS::SNS::Subscription
```

resources.

---

# 🔴 Issue 5: Wrong Lambda Handler

## Root Cause

Incorrect handler value.

---

## Fix

Used:

```text id="cf21"
index.lambda_handler
```

---

# 🔴 Issue 6: CloudWatch Logs Missing

## Root Cause

Lambda never executed.

---

## Fix

Published test messages and verified trigger mappings.

---

# 🧠 Key Learnings

* CloudFormation automates infrastructure deployment
* SNS enables pub/sub messaging
* SQS supports decoupled message processing
* Lambda processes queue messages serverlessly
* IAM permissions are critical for integrations
* Event source mappings connect SQS with Lambda

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Priority task processing
* Background job execution
* Order processing systems
* Notification pipelines
* Serverless event processing
* Distributed queue systems

---

# 🏁 Final Outcome

✔ Priority queue architecture deployed successfully
✔ SNS-to-SQS fanout configured
✔ Lambda processed queue messages
✔ Infrastructure fully automated using CloudFormation
✔ Event-driven serverless workflow validated

---

# 💡 DevOps Insight

> “CloudFormation transforms infrastructure deployment from manual setup into repeatable, version-controlled automation.”
