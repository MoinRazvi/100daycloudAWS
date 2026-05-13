# 🚀 Day 35 – EC2 to Private RDS MySQL Connectivity Using PHP

## 📘 Topic

Deploying a PHP application on EC2 and connecting it securely to a private Amazon RDS MySQL database.

---

# 🎯 Objective

The Nautilus Development Team required:

* A private RDS MySQL database
* Secure EC2 ↔ RDS connectivity
* PHP application deployment
* Browser-based database connectivity validation

This task demonstrates:

* Secure database architecture
* EC2 and RDS integration
* Security group communication
* PHP MySQL connectivity
* SSH key-based authentication

---

# 🏗️ Architecture Overview

```text
User Browser
      ↓
EC2 Instance (Apache + PHP)
      ↓
Security Group Rules
      ↓
Private RDS MySQL Database
```

---

# 🧱 Requirements

| Component     | Value              |
| ------------- | ------------------ |
| RDS Name      | `datacenter-rds`   |
| Engine        | MySQL 8.4.5        |
| DB Class      | `db.t3.micro`      |
| Storage Type  | `gp2`              |
| Storage Size  | `5 GiB`            |
| Database Name | `datacenter_db`    |
| Username      | `datacenter_admin` |
| EC2 Instance  | `datacenter-ec2`   |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Private RDS Instance

Go to:

```text
AWS Console → RDS → Create Database
```

---

## Database Configuration

| Setting       | Value               |
| ------------- | ------------------- |
| Engine        | MySQL               |
| Version       | 8.4.5               |
| Template      | Sandbox / Free Tier |
| DB Identifier | `datacenter-rds`    |
| Username      | `datacenter_admin`  |
| Password      | Custom password     |
| DB Class      | `db.t3.micro`       |
| Storage Type  | `gp2`               |
| Storage Size  | `5 GiB`             |

---

## Connectivity Settings

| Setting          | Value           |
| ---------------- | --------------- |
| Public Access    | No              |
| VPC              | Same as EC2     |
| Initial Database | `datacenter_db` |

---

## Final Step

Click:

```text
Create Database
```

Wait until:

```text
Status = Available
```

---

# 2️⃣ Configure Security Groups

---

## RDS Security Group

Add inbound rule:

| Type         | Port | Source             |
| ------------ | ---- | ------------------ |
| MySQL/Aurora | 3306 | EC2 Security Group |

---

## EC2 Security Group

Add inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

# 3️⃣ Generate SSH Key on aws-client

Check existing key:

```bash
ls /root/.ssh/id_rsa
```

If not present:

```bash
ssh-keygen -t rsa
```

Press Enter for defaults.

---

# 4️⃣ View Public Key

```bash
cat /root/.ssh/id_rsa.pub
```

Copy full output.

---

# 5️⃣ Connect to EC2 Instance

Go to:

```text
AWS Console → EC2 → datacenter-ec2 → Connect
```

Use:

```text
EC2 Instance Connect
```

---

# 6️⃣ Configure Password-less SSH

On EC2:

```bash
mkdir -p /home/ec2-user/.ssh
vi /home/ec2-user/.ssh/authorized_keys
```

Paste public key.

---

## Set Permissions

```bash
chmod 700 /home/ec2-user/.ssh
chmod 600 /home/ec2-user/.ssh/authorized_keys
```

---

# 7️⃣ SSH into EC2 from aws-client

```bash
ssh ec2-user@<EC2-PUBLIC-IP>
```

---

# 8️⃣ Copy PHP File to EC2

From aws-client:

```bash
scp /root/index.php ec2-user@<EC2-PUBLIC-IP>:/var/www/html/
```

---

# 9️⃣ Install Apache & PHP Packages

SSH into EC2 and run:

```bash
sudo yum install -y httpd php php-mysqli
```

---

## Start Apache

```bash
sudo systemctl enable httpd
sudo systemctl start httpd
```

---

# 🔟 Retrieve RDS Endpoint

Go to:

```text
RDS → datacenter-rds → Connectivity & security
```

Copy:

```text
Endpoint
```

Example:

```text
datacenter-rds.xxxxxx.us-east-1.rds.amazonaws.com
```

---

# 1️⃣1️⃣ Modify index.php

Edit:

```bash
vi /var/www/html/index.php
```

Update database details:

```php
<?php
$host = "RDS-ENDPOINT";
$user = "datacenter_admin";
$pass = "PASSWORD";
$db = "datacenter_db";

$conn = new mysqli($host, $user, $pass, $db);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

echo "Connected successfully";
?>
```

---

# 1️⃣2️⃣ Validate Application

Open browser:

```text
http://<EC2-PUBLIC-IP>
```

Expected output:

```text
Connected successfully
```

---

# ✅ Validation Checklist

* ✔ Private RDS created successfully
* ✔ Database configured correctly
* ✔ EC2 connected to RDS
* ✔ PHP application deployed
* ✔ Apache running successfully
* ✔ Browser shows successful DB connection

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Unable to Connect to RDS

### Root Cause

Port 3306 not allowed in RDS security group.

---

### Fix

Add inbound rule:

| Type         | Port | Source             |
| ------------ | ---- | ------------------ |
| MySQL/Aurora | 3306 | EC2 Security Group |

---

## 🔴 Issue 2: Browser Not Opening Website

### Root Cause

HTTP port 80 not allowed.

---

### Fix

Add EC2 inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

## 🔴 Issue 3: `authorized_keys` File Missing

### Problem

File did not exist.

---

### Fix

Create manually:

```bash
mkdir -p /home/ec2-user/.ssh
vi /home/ec2-user/.ssh/authorized_keys
```

---

## 🔴 Issue 4: Permission Denied While Writing in `/root/.ssh`

### Root Cause

No sudo/root permissions on EC2 instance.

---

### Fix

Use:

```text
/home/ec2-user/.ssh/
```

instead of:

```text
/root/.ssh/
```

---

## 🔴 Issue 5: PHP MySQL Driver Missing

### Error

```text
Class 'mysqli' not found
```

---

### Fix

Install:

```bash
sudo yum install -y php-mysqli
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

---

## 🔴 Issue 6: Forgot RDS Password

### Problem

AWS does not display DB password after creation.

---

### Fix

Go to:

```text
RDS → Modify
```

Reset password manually.

---

## 🔴 Issue 7: Could Not Find RDS Endpoint

### Fix

Go to:

```text
RDS → Connectivity & security
```

Copy:

```text
Endpoint
```

---

## 🔴 Issue 8: Used `0.0.0.0/0` for RDS Access

### Problem

RDS became publicly accessible.

---

### Correct Design

Allow MySQL access only from:

```text
EC2 Security Group
```

NOT:

```text
0.0.0.0/0
```

---

# 🧠 Key Learnings

* Private RDS improves database security
* Security groups act as virtual firewalls
* EC2 applications connect using RDS endpoint
* Password-less SSH simplifies automation
* PHP applications require DB drivers

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* WordPress hosting
* PHP applications
* Internal enterprise applications
* Three-tier architectures
* Secure backend database connectivity

---

# 🏁 Final Outcome

✔ Private RDS deployed successfully
✔ EC2 securely connected to MySQL database
✔ PHP application configured correctly
✔ Browser validated successful database connectivity

---
