# 🚀 Day 17 – Create IAM Group & Attach Policy

## 🎯 Objective

Create an **IAM group** and attach an IAM policy to it, enabling **scalable, maintainable, and auditable access control** aligned with AWS best practices.

---

## 🧠 Why This Matters

Managing permissions at the **group level** instead of assigning them directly to users ensures:

* Consistent access control
* Easier onboarding and offboarding
* Reduced risk of permission sprawl

IAM groups are the preferred mechanism for managing **human access** in AWS.

---

## 🛠️ Prerequisites

* Existing IAM users (example: `iamuser_example`)
* Required IAM policy (AWS managed or customer managed)
* IAM permissions to create groups and attach policies

---

## 🖥️ Step-by-Step: Create IAM Group & Attach Policy (AWS Console)

### 1️⃣ Open AWS Console

* Navigate to **[https://console.aws.amazon.com/](https://console.aws.amazon.com/)**
* Sign in with IAM administrative privileges

> 📌 IAM is a global service; region selection does not apply.

---

### 2️⃣ Navigate to IAM

1. Use the top search bar
2. Search for **IAM**
3. Click **IAM (Identity and Access Management)**

---

### 3️⃣ Create IAM Group

* In the left navigation pane, click **User groups**
* Click **Create group**

#### Group Details

* **Group name**:

  ```
  iamgroup_example
  ```

---

### 4️⃣ Attach Policy to Group

Choose one of the following based on requirements:

#### Option A: AWS Managed Policy

* Search for the required policy (example: `AmazonEC2ReadOnlyAccess`)
* Select the policy

#### Option B: Customer Managed Policy

* Search for the custom policy name
* Select the appropriate policy

> 📌 Policies should always reflect **least privilege**.

---

### 5️⃣ Create Group

* Review configuration
* Click **Create group**

The group is now created with the policy attached.

---

## ✅ Validation Checklist

* IAM group exists under **IAM → User groups**
* Correct policy is attached to the group
* No unnecessary policies are attached

---

## ⚠️ Common Operational Mistakes

* Attaching policies directly to users instead of groups
* Overusing broad policies like `AdministratorAccess`
* Creating too many groups without a clear access model

---

## 🧠 Architectural Insight

A clean IAM design uses:

* **Users** for identities
* **Groups** for permission sets
* **Roles** for services and temporary access

This separation simplifies audits and long-term maintenance.

---

## 📌 Day 17 Summary

Day 17 focused on **group-based permission management**, reinforcing scalable and secure IAM practices by attaching policies at the group level.

* Group-based access control as the IAM best practice
* Clear separation of users, groups, and policies
* Avoiding direct policy attachment to users
* Designing IAM for scalability and auditability
---
