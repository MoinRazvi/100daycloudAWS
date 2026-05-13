# 🚀 Day 36 – EC2 + Nginx + Application Load Balancer (ALB)

## 📘 Topic

Deploying an Nginx Web Server Behind an AWS Application Load Balancer

---

# 🎯 Objective

Provision an EC2 instance running Nginx and expose it securely through an Application Load Balancer (ALB).

This lab demonstrates:

* Automated EC2 provisioning
* Nginx web server deployment
* ALB traffic routing
* Target group configuration
* Security group communication design
* High availability architecture basics

---

# 🏗️ Architecture Overview

```text
Internet
    ↓
Application Load Balancer (devops-alb)
    ↓
Target Group (devops-tg)
    ↓
EC2 Instance (devops-ec2)
    ↓
Nginx Web Server
```

---

# 🧱 Requirements

| Component      | Value        |
| -------------- | ------------ |
| Security Group | `devops-sg`  |
| EC2 Name       | `devops-ec2` |
| ALB Name       | `devops-alb` |
| Target Group   | `devops-tg`  |
| Listener Port  | 80           |
| Target Port    | 80           |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Security Group

Go to:

```text
EC2 → Security Groups → Create Security Group
```

## Configuration

| Field | Value       |
| ----- | ----------- |
| Name  | `devops-sg` |
| VPC   | Default VPC |

## Inbound Rule

Add:

| Type | Port | Source                 |
| ---- | ---- | ---------------------- |
| HTTP | 80   | Default Security Group |

> This allows traffic only from the ALB to the EC2 instance.

## Outbound Rule

Keep default:

```text
All traffic
```

---

# 2️⃣ Launch EC2 Instance

Go to:

```text
EC2 → Launch Instance
```

## EC2 Configuration

| Field          | Value        |
| -------------- | ------------ |
| Name           | `devops-ec2` |
| AMI            | Ubuntu       |
| Instance Type  | t2.micro     |
| Security Group | `devops-sg`  |

---

# 3️⃣ Add User Data Script

Under:

```text
Advanced Details → User Data
```

Add:

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

---

# 4️⃣ Verify EC2 Status

Wait until:

```text
Running
```

and:

```text
2/2 Status Checks Passed
```

---

# 5️⃣ Create Target Group

Go to:

```text
EC2 → Target Groups → Create Target Group
```

## Configuration

| Field       | Value       |
| ----------- | ----------- |
| Target Type | Instances   |
| Name        | `devops-tg` |
| Protocol    | HTTP        |
| Port        | 80          |
| VPC         | Default VPC |

## Register Targets

Select:

```text
devops-ec2
```

Click:

```text
Include as pending below
```

Then:

```text
Create target group
```

---

# 6️⃣ Create Application Load Balancer

Go to:

```text
EC2 → Load Balancers → Create Load Balancer
```

Choose:

```text
Application Load Balancer
```

## ALB Configuration

| Field   | Value           |
| ------- | --------------- |
| Name    | `devops-alb`    |
| Scheme  | Internet-facing |
| IP Type | IPv4            |
| VPC     | Default VPC     |

## Availability Zones

Select:

* Minimum 2 AZs
* Public subnets

## Security Group

Attach:

```text
default
```

---

# 7️⃣ Configure Listener

Listener:

| Protocol | Port |
| -------- | ---- |
| HTTP     | 80   |

Forward to:

```text
devops-tg
```

---

# 8️⃣ Modify Default Security Group (ALB)

Since ALB uses default SG:

Add inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

# 9️⃣ Validate Target Health

Go to:

```text
Target Groups → devops-tg → Targets
```

Wait until:

```text
Healthy
```

---

# 🔟 Access Website via ALB DNS

Go to:

```text
EC2 → Load Balancers → devops-alb
```

Copy:

```text
DNS Name
```

Open browser:

```text
http://<ALB-DNS>
```

Expected output:

```text
Welcome to nginx!
```

---

# ✅ Validation Checklist

* ✔ Security group created successfully
* ✔ EC2 launched with user data
* ✔ Nginx installed automatically
* ✔ Target group configured
* ✔ ALB configured correctly
* ✔ Target health = Healthy
* ✔ Website accessible using ALB DNS

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Target Group Unhealthy

### Root Cause

Nginx service not running.

### Fix

SSH into EC2:

```bash
systemctl status nginx
```

Start service:

```bash
systemctl start nginx
```

---

## 🔴 Issue 2: ALB DNS Not Accessible

### Root Cause

Default Security Group missing HTTP access.

### Fix

Add inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

## 🔴 Issue 3: Health Checks Failing

### Root Cause

Port mismatch or Security Group issue.

### Fix

Ensure:

* Target group port = 80
* Nginx listening on port 80
* `devops-sg` allows HTTP from default SG

---

## 🔴 Issue 4: User Data Script Failed

### Root Cause

Incorrect Ubuntu package manager commands.

### Correct Script

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl enable nginx
systemctl start nginx
```

---

## 🔴 Issue 5: EC2 Directly Exposed to Internet

### Problem

Opening HTTP publicly on EC2 Security Group.

### Correct Design

```text
Internet → ALB → EC2
```

NOT:

```text
Internet → EC2 directly
```

---

## 🔴 Issue 6: Target Group Stuck in Initial State

### Root Cause

EC2 instance not fully initialized.

### Fix

Wait until:

```text
2/2 Status Checks Passed
```

before registering target.

---

## 🔴 Issue 7: Wrong Security Group Attached

### Root Cause

ALB attached with incorrect security group.

### Fix

Attach:

```text
default security group
```

for ALB and:

```text
devops-sg
```

for EC2 instance.

---

# 🧠 Key Learnings

* ALB distributes traffic efficiently
* Target groups manage backend health
* Security groups should follow least privilege
* User data enables automated provisioning
* ALB improves scalability and availability
* Backend servers should not be directly internet-facing

---

# 🔍 Real-World Use Cases

This architecture is widely used for:

* Production web applications
* Auto Scaling environments
* Microservices platforms
* Kubernetes ingress routing
* Highly available web hosting

---

# 🏁 Final Outcome

✔ Nginx web server deployed successfully
✔ ALB configured correctly
✔ Traffic routed through target group
✔ Health checks passing successfully
✔ Application accessible via ALB DNS

---
