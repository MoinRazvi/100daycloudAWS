# 🚀 Day 40 – Troubleshooting Public EC2 Internet Access in AWS

## 📘 Topic

Troubleshooting EC2 internet accessibility issues caused by incorrect VPC networking configuration.

---

# 🎯 Objective

The Nautilus Development Team deployed an Nginx application on an EC2 instance:

```text id="t1"
nautilus-ec2
```

inside VPC:

```text id="t2"
nautilus-vpc
```

Even though:

* Security Group allowed HTTP (80)
* EC2 was running properly
* Nginx was installed

the application was still NOT accessible publicly.

The task was to troubleshoot and fix:

* VPC internet connectivity
* Route tables
* Internet Gateway configuration
* Public subnet routing

---

# 🏗️ Architecture Overview

```text id="t3"
User Browser
      ↓
Internet
      ↓
Internet Gateway (IGW)
      ↓
Route Table
      ↓
Public Subnet
      ↓
EC2 Instance (Nginx)
```

---

# 🧱 Components Involved

| Component      | Name           |
| -------------- | -------------- |
| VPC            | `nautilus-vpc` |
| EC2            | `nautilus-ec2` |
| Security Group | `nautilus-sg`  |
| Web Server     | Nginx          |
| Port           | 80             |

---

# 🛠️ Troubleshooting & Implementation Steps

---

# 1️⃣ Verify EC2 Instance Status

Go to:

```text id="t4"
EC2 → Instances
```

Check:

| Validation     | Expected   |
| -------------- | ---------- |
| Instance State | Running    |
| Status Checks  | 2/2 Passed |
| Public IPv4    | Available  |

---

# 2️⃣ Verify Security Group Rules

Go to:

```text id="t5"
EC2 → Security Groups → nautilus-sg
```

---

## Inbound Rules

Ensure:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

## Outbound Rules

Keep:

```text id="t6"
All traffic
```

---

# 3️⃣ Verify Nginx Service

Connect to EC2 instance and run:

```bash id="t7"
systemctl status nginx
```

If not running:

```bash id="t8"
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

# 4️⃣ Verify VPC Internet Gateway

Go to:

```text id="t9"
VPC → Internet Gateways
```

---

## Validation

Ensure:

* Internet Gateway exists
* Attached to:

```text id="t10"
nautilus-vpc
```

---

# 5️⃣ Verify Route Table Configuration

Go to:

```text id="t11"
VPC → Route Tables
```

Select route table associated with public subnet.

---

## Required Route

Ensure route exists:

| Destination | Target           |
| ----------- | ---------------- |
| `0.0.0.0/0` | Internet Gateway |

Example:

```text id="t12"
0.0.0.0/0 → igw-xxxxxxxx
```

---

# 6️⃣ Verify Subnet Association

Under:

```text id="t13"
Route Table → Subnet Associations
```

Ensure public subnet is associated with correct route table.

---

# 7️⃣ Verify Public IP Assignment

Go to:

```text id="t14"
EC2 → nautilus-ec2
```

Ensure:

```text id="t15"
Public IPv4 address
```

exists.

---

# 8️⃣ Verify Network ACLs (If Needed)

Go to:

```text id="t16"
VPC → Network ACLs
```

Ensure:

* inbound allows HTTP
* outbound allows ephemeral traffic

Usually default NACL works.

---

# 9️⃣ Test Website Access

Copy:

```text id="t17"
Public IPv4 Address
```

Open browser:

```text id="t18"
http://<PUBLIC-IP>
```

Expected:

```text id="t19"
Welcome to nginx!
```

---

# ✅ Validation Checklist

* ✔ Internet Gateway attached
* ✔ Route table configured correctly
* ✔ Public subnet associated
* ✔ Security Group allows HTTP
* ✔ EC2 has public IP
* ✔ Nginx running successfully
* ✔ Website accessible publicly

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Security Group Was Correct but Website Still Not Accessible

### Root Cause

VPC route table missing:

```text id="t20"
0.0.0.0/0 → Internet Gateway
```

---

### Fix

Add route manually.

---

## 🔴 Issue 2: Internet Gateway Not Attached

### Root Cause

VPC had no IGW attachment.

---

### Fix

Go to:

```text id="t21"
VPC → Internet Gateways
```

Attach IGW to:

```text id="t22"
nautilus-vpc
```

---

## 🔴 Issue 3: Public Subnet Using Wrong Route Table

### Root Cause

Subnet associated with private route table.

---

### Fix

Associate subnet with:

```text id="t23"
Public Route Table
```

---

## 🔴 Issue 4: Nginx Not Running

### Root Cause

Service inactive.

---

### Fix

```bash id="t24"
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🔴 Issue 5: EC2 Had No Public IP

### Root Cause

Auto-assign public IP disabled.

---

### Fix

Enable:

```text id="t25"
Auto Assign Public IP
```

or attach Elastic IP.

---

## 🔴 Issue 6: Network ACL Blocking Traffic

### Root Cause

Custom NACL denied inbound/outbound traffic.

---

### Fix

Allow:

* HTTP 80
* Ephemeral ports

---

# 🧠 Key Learnings

* Internet access requires BOTH:

  * security group
  * routing configuration

* Internet Gateway enables external connectivity

* Route tables control traffic flow

* Public subnets require:

  * IGW route
  * public IP

* Security groups alone are not sufficient

---

# 🔍 Real-World Use Cases

This troubleshooting pattern is common for:

* Public web servers
* Load balancers
* Bastion hosts
* Production VPC networking
* Cloud migration issues

---

# 🏁 Final Outcome

✔ VPC internet access fixed successfully
✔ Route table configured correctly
✔ EC2 instance publicly accessible
✔ Nginx website reachable on port 80

---
