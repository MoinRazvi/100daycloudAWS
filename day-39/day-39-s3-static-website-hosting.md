# 🚀 Day 39 – Hosting a Static Website Using Amazon S3

## 📘 Topic

Deploying a publicly accessible static website using Amazon S3 Static Website Hosting.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create an S3 bucket
* Enable static website hosting
* Upload website files
* Configure public access
* Host a public website directly from S3

This task demonstrates:

* Static website hosting on AWS
* Public S3 bucket configuration
* S3 bucket policies
* Website deployment without EC2
* Cloud-native web hosting

---

# 🏗️ Architecture Overview

```text id="d1"
User Browser
      ↓
Internet
      ↓
Amazon S3 Static Website Hosting
      ↓
index.html
```

---

# 🧱 Requirements

| Component    | Value                     |
| ------------ | ------------------------- |
| S3 Bucket    | `nautilus-web-1518611717` |
| Index File   | `index.html`              |
| Hosting Type | Static Website Hosting    |
| Access       | Public                    |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create S3 Bucket

Go to:

```text id="d2"
AWS Console → S3 → Create Bucket
```

---

## Bucket Configuration

| Field       | Value                     |
| ----------- | ------------------------- |
| Bucket Name | `nautilus-web-1518611717` |
| Region      | Default                   |

---

# ⚠️ Important

Uncheck:

```text id="d3"
Block all public access
```

Confirm warning checkbox.

---

# 2️⃣ Enable Static Website Hosting

Open bucket:

```text id="d4"
nautilus-web-1518611717
```

Go to:

```text id="d5"
Properties → Static website hosting
```

---

## Configuration

| Setting                | Value                 |
| ---------------------- | --------------------- |
| Static Website Hosting | Enable                |
| Hosting Type           | Host a static website |
| Index Document         | `index.html`          |

Save changes.

---

# 3️⃣ Upload index.html File

On aws-client host verify file:

```bash id="d6"
ls /root/index.html
```

---

## Upload Using AWS Console

Go to:

```text id="d7"
S3 → nautilus-web-1518611717 → Upload
```

Upload:

```text id="d8"
/root/index.html
```

---

# 4️⃣ Configure Bucket Policy for Public Access

Go to:

```text id="d9"
Permissions → Bucket Policy
```

Paste:

```json id="d10"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::nautilus-web-1518611717/*"
    }
  ]
}
```

Save policy.

---

# 5️⃣ Verify Object Permissions

Ensure uploaded file is publicly accessible.

Go to:

```text id="d11"
Objects → index.html
```

Permissions should allow public read access through bucket policy.

---

# 6️⃣ Access Website URL

Go to:

```text id="d12"
Properties → Static Website Hosting
```

Copy:

```text id="d13"
Bucket website endpoint
```

Example:

```text id="d14"
http://nautilus-web-1518611717.s3-website-us-east-1.amazonaws.com
```

Open in browser.

---

# ✅ Expected Output

Website should display contents of:

```text id="d15"
index.html
```

successfully.

---

# ✅ Validation Checklist

* ✔ S3 bucket created
* ✔ Static website hosting enabled
* ✔ index.html uploaded successfully
* ✔ Public access configured
* ✔ Bucket policy applied
* ✔ Website accessible using S3 URL

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Access Denied Error

### Root Cause

Bucket policy missing.

---

### Fix

Add public read bucket policy:

```json id="d16"
"s3:GetObject"
```

for:

```text id="d17"
arn:aws:s3:::nautilus-web-1518611717/*
```

---

## 🔴 Issue 2: Website URL Not Working

### Root Cause

Static website hosting not enabled.

---

### Fix

Enable:

```text id="d18"
Properties → Static Website Hosting
```

---

## 🔴 Issue 3: 403 Forbidden Error

### Root Cause

Public access block still enabled.

---

### Fix

Disable:

```text id="d19"
Block all public access
```

---

## 🔴 Issue 4: index.html Not Loading

### Root Cause

Incorrect index document name.

---

### Fix

Set:

```text id="d20"
index.html
```

exactly in Static Website Hosting configuration.

---

## 🔴 Issue 5: Uploaded Wrong File Path

### Problem

index.html missing in bucket.

---

### Fix

Verify:

```bash id="d21"
ls /root/index.html
```

before upload.

---

## 🔴 Issue 6: Using S3 Object URL Instead of Website Endpoint

### Problem

Page downloaded instead of opening website.

---

### Correct URL

Use:

```text id="d22"
Bucket Website Endpoint
```

NOT:

```text id="d23"
Object URL
```

---

# 🧠 Key Learnings

* S3 can host static websites without EC2
* Bucket policies control public access
* Website endpoints differ from object URLs
* Static hosting is low-cost and scalable
* Public access requires both:

  * bucket policy
  * public access settings

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Portfolio websites
* Landing pages
* Documentation hosting
* Static React/Angular sites
* Internal company portals

---

# 🏁 Final Outcome

✔ Static website hosted successfully on S3
✔ Public access configured correctly
✔ index.html accessible through website URL
✔ AWS-native serverless hosting implemented

---
