# 🚀 Day 16 – Create IAM User

## 🎯 Objective

Create an **AWS IAM user** to establish a distinct identity for accessing AWS resources, reinforcing **identity isolation, access control, and least-privilege principles**.

---

## 🧠 Why This Matters

IAM users represent **human or application identities** in AWS. Creating individual users instead of sharing root credentials is essential for:

* Accountability and auditing
* Secure access management
* Controlled permission assignment

Identity management is the foundation of a secure AWS environment.

---

## 🛠️ Prerequisites

* AWS account with administrative or IAM permissions
* Access to the AWS Management Console

---

## 🖥️ Step-by-Step: Create IAM User (AWS Console)

### 1️⃣ Open AWS Console

* Go to **[https://console.aws.amazon.com/](https://console.aws.amazon.com/)**
* Sign in using an account with IAM privileges

> 📌 IAM is a **global service**, so region selection does not matter.

---

### 2️⃣ Navigate to IAM

1. Use the top search bar
2. Search for **IAM**
3. Click **IAM (Identity and Access Management)**

---

### 3️⃣ Go to Users

* In the left navigation pane, click **Users**
* Click **Create user**

---

### 4️⃣ Configure User Details

* **User name**:

  ```
  iamuser_example
  ```

* **Access type**:

  * Leave **AWS Management Console access** unchecked
  * Do not generate access keys unless explicitly required

This creates the user without permissions or credentials by default.

* Click **Next**

---

### 5️⃣ Set Permissions

* Choose:

  ```
  Add user to group
  ```
* Do not select any group (unless specified)

> 📌 Permissions should be granted deliberately, not by default.

* Click **Next**

---

### 6️⃣ Review and Create User

* Review the configuration
* Click **Create user**

You should see a confirmation message indicating successful user creation.

---

## ✅ Validation Checklist

* IAM user exists under **IAM → Users**
* User has no permissions unless intentionally assigned
* No console password or access keys created unintentionally

---

## ⚠️ Common Operational Mistakes

* Sharing root credentials instead of creating users
* Granting permissions at user creation without review
* Creating access keys unnecessarily

---

## 🧠 Architectural Insight

IAM users should represent **people, not roles**. Permissions should be assigned through **groups or roles**, enabling scalable and auditable access control.

---

## 📌 Day 16 Summary

Day 16 focused on creating an **IAM user as a discrete identity**, reinforcing secure access practices and laying the groundwork for structured permission management.

* IAM as the foundation of AWS security
* Avoiding root usage and shared credentials
* Creating identities without permissions by default
* Preparing for scalable access via groups and roles
---

📅 **Next Up: Day 17 – Create IAM Group and Assign Permissions**
