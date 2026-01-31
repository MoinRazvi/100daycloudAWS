# 🚀 Day 26 – Configuring an EC2 Instance as a Web Server with Nginx

## 🎯 Objective

Launch an **EC2 instance** and configure it as a **web server using Nginx** via **user data**, ensuring the service is automatically installed, started, and accessible over the internet.

This day focuses on **bootstrapping compute with automation**, a core DevOps practice.

---

## 🧠 Why This Matters

Manual server configuration does not scale. Using **EC2 user data** enables:

* Consistent server builds
* Faster provisioning
* Reduced configuration drift

Nginx is a lightweight, high-performance web server commonly used as a **reverse proxy, load balancer, or application front-end**.

---

## 🛠️ Prerequisites

* AWS account with EC2 permissions
* Public subnet with internet access
* Security group allowing HTTP (port 80)

---

## 🖥️ Step-by-Step: Launch EC2 Instance

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Choose required region (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2

* Search **EC2** → Click **EC2**

---

### 3️⃣ Launch Instance

* Click **Launch instances**

Configure:

* **Name**:

  ```
  devops-ec2
  ```
* **AMI**: Ubuntu Server LTS (20.04 / 22.04)
* **Instance type**:

  ```
  t2.micro
  ```

---

## 🔐 Network & Security Configuration

* **VPC**: Public VPC
* **Subnet**: Public subnet
* **Auto-assign public IP**: Enabled

Security Group inbound rules:

| Type | Port | Source                         |
| ---- | ---- | ------------------------------ |
| HTTP | 80   | 0.0.0.0/0                      |
| SSH  | 22   | Your IP / 0.0.0.0/0 (lab only) |

---

## 🧩 Configure User Data (Critical Step)

Scroll to **Advanced details → User data** and paste:

```bash
#!/bin/bash
apt update -y
apt install -y nginx
systemctl start nginx
systemctl enable nginx
```

This script:

* Updates the OS
* Installs Nginx
* Starts the service
* Ensures Nginx runs on reboot

---

## 🚀 Launch Instance

* Select key pair (or proceed without key pair if restricted)
* Click **Launch instance**

Wait until:

```
Instance state: Running
```

---

## 🌐 Verification Steps

### 1️⃣ Verify Nginx Service

Open a browser and navigate to:

```
http://<EC2-PUBLIC-IP>
```

You should see the **Nginx welcome page**.

---

## ✅ Validation Checklist

* EC2 instance is **Running**
* Public IPv4 address assigned
* Port 80 open in security group
* Nginx page loads successfully

---

## ⚠️ Common Operational Mistakes

* Using Ubuntu commands on Amazon Linux or vice versa
* Forgetting to open port 80
* Missing `#!/bin/bash` in user data
* Editing user data after launch (does not re-run)

---

## 🧠 Architectural Insight

User data is ideal for **initial bootstrapping**, but long-term configuration should be managed via **configuration management tools** (Ansible, SSM, etc.) or **immutable images**.

---

## 📌 Day 26 Summary

Day 26 focused on provisioning an **EC2-based web server using Nginx**, reinforcing automation-first thinking and foundational web hosting concepts in AWS.

* Automation-first mindset using EC2 user data
* Correct OS-specific package management (Ubuntu + apt)
* Security group fundamentals for web workloads
* Validation via public access (Nginx welcome page)
* Clear distinction between bootstrapping vs configuration management
---
