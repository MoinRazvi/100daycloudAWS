# 🚀 Day 06 – Launch EC2 Instance (Compute Foundation)

## 🎯 Objective

Launch an **Amazon EC2 instance** using appropriate defaults and configurations, establishing a reliable compute foundation for application workloads while following **clean, production-ready practices**.

---

## 🧠 Why This Matters

EC2 is the core compute service in AWS. How an instance is launched—AMI choice, instance type, networking, and tagging—directly impacts **security, cost, performance, and operability**.

Launching EC2 correctly from day one avoids technical debt later.

---

## 🛠️ Prerequisites

* AWS account access
* Existing VPC, subnet, and security group
* Basic understanding of Linux and SSH

---

## 🖥️ Step-by-Step: Launch an EC2 Instance (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the required region (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2 Dashboard

1. Open **AWS Console**
2. Search for **EC2**
3. Click **EC2**

---

### 3️⃣ Launch Instance

* Click **Instances** in the left menu
* Click **Launch instances**

---

### 4️⃣ Name and Tag the Instance

Under **Name and tags**:

```
Name: xfusion-ec2
```

Consistent naming simplifies monitoring, automation, and troubleshooting.

---

### 5️⃣ Choose an AMI

Under **Application and OS Images**:

* Select **Amazon Linux** or **Ubuntu LTS**

Choose stable, widely supported AMIs for predictable behavior.

---

### 6️⃣ Select Instance Type

Under **Instance type**:

```
t2.micro
```

This instance type is suitable for labs, testing, and lightweight workloads.

---

### 7️⃣ Configure Key Pair

* Select an existing key pair
  **or**
* Create a new key pair if required

Key pairs enable secure, passwordless SSH access.

---

### 8️⃣ Configure Networking

Under **Network settings**:

* **VPC**: Default or project VPC
* **Subnet**: Select the appropriate subnet
* **Security group**: Attach an existing security group

Ensure the security group allows required traffic only.

---

### 9️⃣ Review and Launch

* Review configuration summary
* Click **Launch instance**

Wait until:

```
Instance state: Running
```

---

## ✅ Validation Checklist

* Instance is in **Running** state
* Correct AMI and instance type selected
* Proper subnet and security group attached
* Name tag applied correctly

---

## ⚠️ Common Operational Mistakes

* Launching instances without tags
* Using overly large instance types by default
* Attaching the wrong security group
* Ignoring subnet placement

---

## 🧠 Architectural Insight

Compute should always be treated as **ephemeral**. Design systems so instances can be stopped, replaced, or recreated without data loss.

---

## 📌 Day 06 Summary

Day 06 focused on launching an **EC2 instance with clean defaults**, reinforcing best practices around compute provisioning, networking alignment, and operational readiness.

* EC2 as ephemeral compute, not a pet server
* Importance of correct AMI, instance type, subnet, and SG selection
* Clean tagging and launch hygiene
* Avoiding common operational mistakes seen in real projects
---
