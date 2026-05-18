# 🚀 Day 37 – EC2 to Private S3 Bucket Access Using IAM Role

## 📘 Topic

Securely connecting an EC2 instance to a private Amazon S3 bucket using IAM Roles and Policies.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create a private S3 bucket
* Configure IAM policy and role
* Attach IAM role to EC2
* Enable EC2 to upload and retrieve files securely from S3
* Avoid hardcoded AWS credentials

This task demonstrates:

* IAM Roles for EC2
* Secure S3 access
* Least privilege IAM policies
* EC2 ↔ S3 integration
* Password-less SSH setup

---

# 🏗️ Architecture Overview

```text id="w1c6qa"
User (aws-client)
        ↓ SSH
EC2 Instance (datacenter-ec2)
        ↓ IAM Role
IAM Role (datacenter-role)
        ↓ IAM Policy
Private S3 Bucket
(datacenter-s3-886264053901)
```

---

# 🧱 Requirements

| Component     | Value                            |
| ------------- | -------------------------------- |
| EC2 Instance  | `datacenter-ec2`                 |
| S3 Bucket     | `datacenter-s3-886264053901`     |
| IAM Role      | `datacenter-role`                |
| Access Needed | PutObject, GetObject, ListBucket |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create SSH Key Pair on aws-client

Generate new key:

```bash id="s1"
ssh-keygen -t rsa
```

Press Enter for defaults.

---

# 2️⃣ View Public Key

```bash id="s2"
cat /root/.ssh/id_rsa.pub
```

Copy full output.

---

# 3️⃣ Add Public Key to EC2 Instance

Go to:

```text id="s3"
AWS Console → EC2 → datacenter-ec2 → Connect
```

Use:

```text id="s4"
EC2 Instance Connect
```

---

## On EC2 Instance

Create SSH directory:

```bash id="s5"
mkdir -p /root/.ssh
```

Create file:

```bash id="s6"
vi /root/.ssh/authorized_keys
```

Paste copied public key.

---

## Set Permissions

```bash id="s7"
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```

---

# 4️⃣ Create Private S3 Bucket

Go to:

```text id="s8"
AWS Console → S3 → Create Bucket
```

---

## Configuration

| Field               | Value                        |
| ------------------- | ---------------------------- |
| Bucket Name         | `datacenter-s3-886264053901` |
| Region              | Default                      |
| Block Public Access | Enabled                      |

---

## Important

Keep bucket:

```text id="s9"
Private
```

Do NOT enable:

* public ACLs
* bucket public access

---

# 5️⃣ Create IAM Policy

Go to:

```text id="s10"
IAM → Policies → Create Policy
```

Choose:

```text id="s11"
JSON
```

Paste:

```json id="s12"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::datacenter-s3-886264053901/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::datacenter-s3-886264053901"
    }
  ]
}
```

---

## Policy Name

```text id="s13"
datacenter-s3-policy
```

Create policy.

---

# 6️⃣ Create IAM Role

Go to:

```text id="s14"
IAM → Roles → Create Role
```

---

## Trusted Entity

Choose:

```text id="s15"
AWS Service
```

Use Case:

```text id="s16"
EC2
```

---

## Attach Policy

Select:

```text id="s17"
datacenter-s3-policy
```

---

## Role Name

```text id="s18"
datacenter-role
```

Create role.

---

# 7️⃣ Attach Role to EC2 Instance

Go to:

```text id="s19"
EC2 → datacenter-ec2
```

Actions:

```text id="s20"
Security → Modify IAM Role
```

Attach:

```text id="s21"
datacenter-role
```

Save.

---

# 8️⃣ SSH into EC2 Instance

From aws-client:

```bash id="s22"
ssh root@<EC2-PUBLIC-IP>
```

---

# 9️⃣ Create Test File

On EC2:

```bash id="s23"
echo "KKE AWS Labs" > test.txt
```

---

# 🔟 Upload File to S3

```bash id="s24"
aws s3 cp test.txt s3://datacenter-s3-886264053901/
```

Expected:

```text id="s25"
upload: ./test.txt to s3://datacenter-s3-886264053901/test.txt
```

---

# 1️⃣1️⃣ Verify Uploaded File

```bash id="s26"
aws s3 ls s3://datacenter-s3-886264053901/
```

Expected output:

```text id="s27"
test.txt
```

---

# ✅ Validation Checklist

* ✔ Private S3 bucket created
* ✔ IAM policy configured
* ✔ IAM role attached to EC2
* ✔ SSH access configured
* ✔ EC2 uploaded file successfully
* ✔ S3 object listing working

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Access Denied While Uploading

### Error

```text id="s28"
AccessDenied
```

---

### Root Cause

IAM role not attached properly.

---

### Fix

Attach:

```text id="s29"
datacenter-role
```

to:

```text id="s30"
datacenter-ec2
```

---

## 🔴 Issue 2: Bucket Still Public

### Root Cause

Public access settings disabled accidentally.

---

### Fix

Enable:

```text id="s31"
Block all public access
```

---

## 🔴 Issue 3: `aws s3 cp` Command Fails

### Root Cause

AWS CLI not installed/configured.

---

### Fix

Verify:

```bash id="s32"
aws --version
```

---

## 🔴 Issue 4: Wrong Bucket ARN in Policy

### Problem

Policy applied to incorrect resource.

---

### Correct ARN

```text id="s33"
arn:aws:s3:::datacenter-s3-886264053901
```

and:

```text id="s34"
arn:aws:s3:::datacenter-s3-886264053901/*
```

---

## 🔴 Issue 5: Permission Denied During SSH

### Root Cause

Public key not added properly.

---

### Fix

Verify:

```text id="s35"
/root/.ssh/authorized_keys
```

contains:

```text id="s36"
id_rsa.pub contents
```

---

## 🔴 Issue 6: Forgot to Attach Role to EC2

### Symptom

Even correct IAM policy fails.

---

### Fix

Go to:

```text id="s37"
EC2 → Actions → Security → Modify IAM Role
```

Attach:

```text id="s38"
datacenter-role
```

---

# 🧠 Key Learnings

* IAM Roles are more secure than access keys
* S3 bucket policies and IAM policies work together
* EC2 assumes IAM role automatically
* Least privilege access improves security
* Private S3 buckets are production best practice

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Application log storage
* Backup systems
* Static file uploads
* Media storage
* DevOps automation pipelines

---

# 🏁 Final Outcome

✔ Private S3 bucket created successfully
✔ IAM role-based access configured
✔ EC2 securely accessed S3
✔ File upload and listing validated successfully

---
