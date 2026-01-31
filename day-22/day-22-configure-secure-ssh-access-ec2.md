# 🚀 Day 22 – Configuring Secure SSH Access to an EC2 Instance

## 🎯 Objective

Configure **secure SSH access** to an EC2 instance, enabling administrators to connect safely while minimizing exposure and following **industry-standard security practices**.

This day focuses on **access security**, which is critical for protecting compute resources from unauthorized access.

---

## 🧠 Why This Matters

SSH is one of the most commonly targeted access methods in cloud environments. Improper SSH configuration can lead to:

* Brute-force attacks
* Credential compromise
* Full instance takeover

Secure SSH access ensures:

* Controlled administrative entry
* Strong authentication
* Reduced attack surface

---

## 🛠️ Prerequisites

* A running EC2 instance (example: `xfusion-pub-ec2` or `devops-ec2`)
* Public IPv4 address assigned to the instance
* Security Group associated with the instance

---

## 🖥️ Step-by-Step: Configure Secure SSH Access

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Choose the region where the EC2 instance is running (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2 Instances

1. Open **AWS Console**
2. Search for **EC2**
3. Click **Instances**

---

### 3️⃣ Verify Security Group SSH Rule

* Select the EC2 instance
* Go to **Security → Security groups**
* Click the attached security group
* Under **Inbound rules**, ensure:

| Type | Port | Source                 |
| ---- | ---- | ---------------------- |
| SSH  | 22   | Your IP / Trusted CIDR |

> 🔐 For labs, `0.0.0.0/0` may be acceptable. In production, always restrict SSH to known IPs or bastion hosts.

---

### 4️⃣ Verify Key Pair Association

* Select the EC2 instance
* Go to **Details** tab
* Confirm **Key pair name** is set

> 📌 SSH access requires a key pair unless access is provided via AWS SSM.

---

## 🔑 Step-by-Step: Connect via SSH

### 5️⃣ Prepare Key Pair (Local Machine)

Ensure correct permissions:

```bash
chmod 400 your-key.pem
```

---

### 6️⃣ Connect to EC2 Instance

Use the public IP:

For **Ubuntu**:

```bash
ssh -i your-key.pem ubuntu@<PUBLIC-IP>
```

For **Amazon Linux**:

```bash
ssh -i your-key.pem ec2-user@<PUBLIC-IP>
```

---

## ✅ Validation Checklist

* Instance reachable via SSH
* Security Group restricts SSH appropriately
* Private key permissions are secure

---

## ⚠️ Common Operational Mistakes

* Leaving SSH open to the entire internet
* Incorrect SSH username
* Wrong file permissions on key pair
* Attempting SSH without a public IP

---

## 🧠 Architectural Insight

For production systems, SSH should be minimized or eliminated using:

* AWS Systems Manager Session Manager
* Bastion hosts
* VPN or Direct Connect

This reduces reliance on exposed ports entirely.

---

## 📌 Day 22 Summary

Day 22 focused on establishing **secure SSH access** to an EC2 instance, reinforcing best practices around authentication, network restriction, and operational security.

* SSH as a high-risk access vector if misconfigured
* Security Group hardening for port 22
* Correct SSH users for Ubuntu vs Amazon Linux
* Key pair hygiene and file permissions
* Forward-looking alternatives (SSM, bastion, VPN)
---
