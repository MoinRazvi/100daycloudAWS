# 🚀 Day 10 – Attach Elastic IP to EC2 Instance

## 🎯 Objective

Attach an **Elastic IP (EIP)** to an EC2 instance to provide a **stable, persistent public IP address**, ensuring consistent external access for applications and integrations.

---

## 🧠 Why This Matters

By default, EC2 public IPs can change when an instance is stopped and started. Elastic IPs decouple the public endpoint from the instance lifecycle, which is essential for:

* Stable application endpoints
* DNS mappings
* Third-party integrations and allowlists

Elastic IPs should be used deliberately and released when not required.

---

## 🛠️ Prerequisites

* An existing EC2 instance (example: `devops-ec2`)
* Permission to allocate and associate Elastic IPs
* Instance running in a public subnet with an Internet Gateway

---

## 🖥️ Step-by-Step: Attach Elastic IP (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the region where the EC2 instance exists (example: **us-east-1**)

---

### 2️⃣ Open EC2 Service

1. Open **AWS Console**
2. Search for **EC2**
3. Click **EC2**

---

### 3️⃣ Allocate a New Elastic IP

* In the left navigation pane, go to:

  ```
  Network & Security → Elastic IPs
  ```
* Click **Allocate Elastic IP address**
* Leave defaults unchanged
* Click **Allocate**

---

### 4️⃣ Tag the Elastic IP

* Select the newly allocated Elastic IP
* Go to **Tags → Manage tags**
* Add:

  ```
  Key: Name
  Value: devops-eip
  ```
* Save changes

Tagging helps with cost tracking and resource clarity.

---

### 5️⃣ Associate Elastic IP with EC2 Instance

* With the Elastic IP selected, click **Actions**
* Choose **Associate Elastic IP address**

Configure association:

* **Resource type**: Instance

* **Instance**: devops-ec2

* **Private IP**: Leave default

* Click **Associate**

---

## ✅ Validation Checklist

* Elastic IP status shows **In use**
* Associated instance is **devops-ec2**
* EC2 public IPv4 address matches the Elastic IP

---

## ⚠️ Common Operational Mistakes

* Forgetting to associate the Elastic IP after allocation
* Leaving unused Elastic IPs allocated (incurs cost)
* Associating EIP to instances in private subnets

---

## 🧠 Architectural Insight

Elastic IPs are account-level resources. Treat them as **static endpoints**, not permanent attachments—especially in architectures that favor load balancers or DNS-based routing.

---

## 📌 Day 10 Summary

Day 10 focused on assigning a **stable public IP address** to an EC2 instance using Elastic IP, reinforcing best practices for reliable connectivity and controlled exposure.

* Why static public IPs matter for reliability
* Decoupling instance lifecycle from public endpoints
* Proper tagging and cost awareness
* When to use Elastic IPs vs load balancers
---
