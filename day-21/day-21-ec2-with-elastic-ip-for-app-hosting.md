# 🚀 Day 21 – Setting Up an EC2 Instance with an Elastic IP for Application Hosting

## 🎯 Objective

Launch an **EC2 instance** and associate an **Elastic IP (EIP)** to provide a **stable, persistent public endpoint** for application hosting, ensuring reliability across instance restarts.

This setup is commonly used for:

* Small public-facing applications
* Bastion hosts
* Legacy services requiring fixed IPs

---

## 🧠 Why This Matters

By default, EC2 public IPs can change when an instance is stopped and started. Elastic IPs decouple **network identity from compute lifecycle**, enabling:

* Consistent DNS mapping
* Firewall and allowlist stability
* Predictable application access

Elastic IPs must be managed carefully to avoid unnecessary costs.

---

## 🛠️ Prerequisites

* AWS account with EC2 permissions
* Existing VPC and subnet (public)
* Security group allowing required inbound traffic

---

## 🖥️ Step-by-Step: Launch EC2 Instance

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Choose the required region (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2

1. Open **AWS Console**
2. Search for **EC2**
3. Click **EC2**

---

### 3️⃣ Launch Instance

* Click **Instances → Launch instances**

Configure:

* **Name**:

  ```
  devops-ec2
  ```
* **AMI**: Ubuntu LTS or Amazon Linux
* **Instance type**:

  ```
  t2.micro
  ```

---

### 🔐 4️⃣ Network & Security

Under **Network settings**:

* **VPC**: Public VPC
* **Subnet**: Public subnet
* **Auto-assign public IP**: Enable (if subnet doesn’t already)

Create / select security group:

Inbound rules:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |
| SSH  | 22   | 0.0.0.0/0 |

> 🔐 Restrict SSH in production; open access is acceptable for lab purposes.

---

### 5️⃣ Launch Instance

* Select a key pair **or proceed without key pair** (lab-dependent)
* Click **Launch instance**

Wait until:

```
Instance state: Running
```

---

## 🌐 Step-by-Step: Allocate and Attach Elastic IP

### 6️⃣ Allocate Elastic IP

* EC2 → **Network & Security → Elastic IPs**
* Click **Allocate Elastic IP address**
* Leave defaults
* Click **Allocate**

---

### 7️⃣ Tag Elastic IP

* Select the allocated EIP
* **Tags → Manage tags**

```
Key: Name
Value: devops-eip
```

Save changes.

---

### 8️⃣ Associate Elastic IP with EC2

* With EIP selected → **Actions → Associate Elastic IP address**

Configure:

* **Resource type**: Instance

* **Instance**: devops-ec2

* **Private IP**: Default

* Click **Associate**

---

## ✅ Validation Checklist

* EC2 instance is **Running**
* Elastic IP status is **In use**
* EC2 public IPv4 equals Elastic IP
* Security group allows required ports

---

## ⚠️ Common Operational Mistakes

* Leaving Elastic IPs allocated but unused (cost)
* Associating EIP to instances in private subnets
* Forgetting to tag Elastic IPs

---

## 🧠 Architectural Insight

Elastic IPs are best suited for **specific fixed-endpoint use cases**. In scalable architectures, prefer **Application Load Balancers with DNS** instead of binding traffic to single instances.

---

## 📌 Day 21 Summary

Day 21 focused on deploying an **EC2 instance with a persistent public endpoint** using Elastic IP, reinforcing reliable application access and network lifecycle management.

* Why Elastic IP ≠ default public IP
* Decoupling network identity from EC2 lifecycle
* Correct sequence: EC2 → EIP → Associate
* Cost awareness and tagging discipline
* When to use EIP vs ALB (architectural thinking)
---
