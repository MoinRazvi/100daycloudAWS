# 🚀 Day 02 – Create a Security Group (AWS)

## 🎯 Objective

Design and create an **AWS Security Group** to control inbound and outbound traffic for EC2 instances, following **least-privilege and production-grade DevOps practices**.

---

## 🧠 Why Security Groups Matter (Senior DevOps Perspective)

Security Groups act as **stateful virtual firewalls** at the instance level. In real-world AWS environments, poorly designed security groups are one of the **top causes of security incidents and outages**.

Key characteristics:

* Stateful (return traffic is automatically allowed)
* Instance-level protection
* Evaluated before traffic reaches the OS

A well-defined security group:

* Minimizes attack surface
* Enforces clear network boundaries
* Supports scalability and automation

---

## 🛠️ Prerequisites

* Active AWS account
* Access to AWS Management Console
* Basic understanding of TCP/IP and ports

---

## 🖥️ Step-by-Step: Create a Security Group (AWS Console)

### 1️⃣ Select Correct Region

* Top-right corner of AWS Console
* Select the required region (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2 Security Groups

1. Open **AWS Console**
2. Search for **EC2**
3. In the left menu, click:

   ```
   Network & Security → Security Groups
   ```

---

### 3️⃣ Create Security Group

* Click **Create security group**

#### Basic Details

* **Security group name**:

  ```
  devops-web-sg
  ```
* **Description**:

  ```
  Allow HTTP and SSH access for web servers
  ```
* **VPC**: Select **Default VPC** (or project-specific VPC)

---

### 4️⃣ Configure Inbound Rules

Add the following rules:

| Type | Protocol | Port | Source               | Purpose             |
| ---- | -------- | ---- | -------------------- | ------------------- |
| SSH  | TCP      | 22   | Your IP / Bastion SG | Secure admin access |
| HTTP | TCP      | 80   | 0.0.0.0/0            | Public web traffic  |

> 🔐 **Best Practice**: Never expose SSH (22) to `0.0.0.0/0` in production.

---

### 5️⃣ Configure Outbound Rules

* Leave **default outbound rule**:

  * Allow all traffic (`0.0.0.0/0`)

This supports package downloads, updates, and external API calls.

---

### 6️⃣ Create Security Group

* Click **Create security group**

You should see:

```
Security group devops-web-sg created successfully
```

---

## ✅ Verification Checklist

* Security group exists
* Correct VPC selected
* Inbound rules follow least privilege
* Outbound rules intact

---

## ⚠️ Common Mistakes (Real-World)

* Opening SSH to the world (`0.0.0.0/0`)
* Using one security group for all workloads
* Forgetting to restrict ALB → EC2 traffic
* Treating security groups like NACLs

---

## 🧠 Architect-Level Insight

> Security Groups should be **role-based**, not instance-based. Design them around *what the resource does*, not where it runs.

---

## 📌 Day 02 Summary

Day 02 focused on designing and creating a **secure, production-ready AWS Security Group**, reinforcing the importance of network security as a foundational DevOps responsibility.

