# 🚀 Day 20 – Create IAM Role for EC2 with Policy Attachment

## 🎯 Objective

Create an **IAM role for Amazon EC2** and attach an IAM policy to enable **secure, temporary, and credential-free access** from EC2 instances to AWS services.

---

## 🧠 Why This Matters

IAM roles are the **recommended mechanism** for granting permissions to AWS services. For EC2, roles eliminate the need to store long‑lived access keys on instances and provide:

* Automatic credential rotation
* Reduced blast radius
* Strong auditability via STS

Roles are fundamental to secure, production-grade AWS architectures.

---

## 🛠️ Prerequisites

* AWS account with IAM administrative permissions
* An existing IAM policy (AWS managed or customer managed)
* Basic understanding of trust relationships

---

## 🖥️ Step-by-Step: Create IAM Role for EC2 (AWS Console)

### 1️⃣ Open AWS Console

* Navigate to **[https://console.aws.amazon.com/](https://console.aws.amazon.com/)**
* Sign in with appropriate IAM privileges

> 📌 IAM is a global service; region selection does not apply.

---

### 2️⃣ Navigate to IAM

1. Use the top search bar
2. Search for **IAM**
3. Click **IAM (Identity and Access Management)**

---

### 3️⃣ Create a New Role

* In the left navigation pane, click **Roles**
* Click **Create role**

---

### 4️⃣ Select Trusted Entity

* **Trusted entity type**: AWS service
* **Use case**: EC2

This establishes a trust relationship allowing EC2 to assume the role.

* Click **Next**

---

### 5️⃣ Attach Permissions Policy

* Search and select the required policy (example: `iampolicy_kareem` or `AmazonEC2ReadOnlyAccess`)

> 📌 Policies should follow the **principle of least privilege**.

* Click **Next**

---

### 6️⃣ Name and Create the Role

* **Role name**:

  ```
  iamrole_kareem
  ```

* (Optional) Description:

  ```
  IAM role for EC2 to access AWS services securely
  ```

* Click **Create role**

---

## ✅ Validation Checklist

* Role exists under **IAM → Roles**
* Trusted entity is **ec2.amazonaws.com**
* Correct policy is attached

---

## ⚠️ Common Operational Mistakes

* Using IAM users instead of roles on EC2
* Attaching overly broad policies
* Confusing role trust policy with permission policy

---

## 🧠 Architectural Insight

EC2 instances should **never** use long-lived access keys. IAM roles provide short-lived credentials via STS, making them the default and secure choice for service-to-service access.

---

## 📌 Day 20 Summary

Day 20 focused on creating an **IAM role for EC2 and attaching a policy**, reinforcing best practices for secure, scalable, and maintainable access control in AWS.

* Using IAM roles instead of access keys on EC2
* Trust relationships vs permission policies (clear separation)
* Least-privilege policy attachment
* Secure, scalable service-to-service access via STS
---
