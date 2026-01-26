# 🚀 Day 18 – Create Read-Only IAM Policy for EC2 Console Access

## 🎯 Objective

Create a **custom IAM policy** that provides **read-only access to the Amazon EC2 console**, allowing users to view instances, AMIs, and snapshots without granting modification privileges.

---

## 🧠 Why This Matters

Read-only access is a common requirement for:

* Auditors
* Support engineers
* Monitoring and troubleshooting roles

Custom policies ensure access is **precisely scoped**, avoiding the risks of overly broad managed policies while maintaining visibility into infrastructure.

---

## 🛠️ Prerequisites

* AWS account with IAM administrative permissions
* Understanding of basic IAM policy structure (Effect, Action, Resource)

---

## 🖥️ Step-by-Step: Create Read-Only EC2 IAM Policy (AWS Console)

### 1️⃣ Open AWS Console

* Navigate to **[https://console.aws.amazon.com/](https://console.aws.amazon.com/)**
* Sign in with appropriate IAM privileges

> 📌 IAM is a global service; region selection does not apply.

---

### 2️⃣ Navigate to IAM Service

1. Use the top search bar
2. Search for **IAM**
3. Click **IAM (Identity and Access Management)**

---

### 3️⃣ Go to Policies

* In the left navigation pane, click **Policies**
* Click **Create policy**

---

### 4️⃣ Choose Policy Editor

* Select the **JSON** tab

Using JSON ensures precise control over permissions.

---

### 5️⃣ Define the Policy Document

Replace the default content with the following:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:DescribeImages",
        "ec2:DescribeSnapshots",
        "ec2:DescribeVolumes",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
```

This policy allows visibility into EC2 resources without modification rights.

* Click **Next**

---

### 6️⃣ Name and Create the Policy

* **Policy name**:

  ```
  ec2-readonly-policy
  ```

* **Description**:

  ```
  Read-only access to EC2 instances, AMIs, and snapshots
  ```

* Click **Create policy**

---

## ✅ Validation Checklist

* Policy appears under **IAM → Policies**
* Policy type is **Customer managed**
* Policy JSON contains only `Describe*` actions

---

## ⚠️ Common Operational Mistakes

* Using broad managed policies unnecessarily
* Accidentally including write actions
* Attaching policies directly to users instead of groups

---

## 🧠 Architectural Insight

Well-designed IAM policies grant **visibility without control**, supporting operational transparency while maintaining strict change boundaries.

---

## 📌 Day 18 Summary

Day 18 focused on creating a **custom, least-privilege IAM policy** to provide read-only access to the EC2 console, reinforcing secure and auditable access design.

* Designing custom IAM policies instead of overusing managed ones
* Applying least privilege with Describe* actions only
* Separating visibility from control
* Building IAM for audits, support, and monitoring roles
---
