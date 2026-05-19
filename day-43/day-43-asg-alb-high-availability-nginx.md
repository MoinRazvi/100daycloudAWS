# 🚀 Day 43 – High Availability Web Application Using ASG + ALB

## 📘 Topic

Deploying a highly available and auto-scalable Nginx web application using AWS Auto Scaling Group (ASG) and Application Load Balancer (ALB).

---

# 🎯 Objective

The DevOps team needs to:

* Create an EC2 Launch Template
* Automatically install Nginx using User Data
* Configure Auto Scaling Group
* Create an Application Load Balancer
* Configure Target Group health checks
* Enable dynamic scaling based on CPU utilization

This task demonstrates:

* Auto Scaling architecture
* Load balancing
* High availability setup
* Dynamic scaling policies
* Automated EC2 provisioning

---

# 🏗️ Architecture Overview

```text id="a1"
User Browser
        ↓
Internet
        ↓
Application Load Balancer (devops-alb)
        ↓
Target Group (devops-tg)
        ↓
Auto Scaling Group (devops-asg)
        ↓
EC2 Instances (Nginx)
```

---

# 🧱 Requirements

| Component        | Value                    |
| ---------------- | ------------------------ |
| Launch Template  | `devops-launch-template` |
| ASG              | `devops-asg`             |
| Target Group     | `devops-tg`              |
| ALB              | `devops-alb`             |
| Min Instances    | 1                        |
| Desired Capacity | 1                        |
| Max Instances    | 2                        |
| Target CPU       | 50%                      |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Security Group

Go to:

```text id="a2"
EC2 → Security Groups → Create Security Group
```

---

## Configuration

| Field | Value       |
| ----- | ----------- |
| Name  | `devops-sg` |
| VPC   | Default     |

---

## Inbound Rule

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

## Outbound Rule

Keep:

```text id="a3"
All traffic
```

---

# 2️⃣ Create Launch Template

Go to:

```text id="a4"
EC2 → Launch Templates → Create Launch Template
```

---

## Configuration

| Field          | Value                    |
| -------------- | ------------------------ |
| Template Name  | `devops-launch-template` |
| AMI            | Amazon Linux 2           |
| Instance Type  | `t2.micro`               |
| Security Group | `devops-sg`              |

---

# 3️⃣ Add User Data Script

Under:

```text id="a5"
Advanced Details → User Data
```

Add:

```bash id="a6"
#!/bin/bash
yum update -y
amazon-linux-extras install nginx1 -y
systemctl enable nginx
systemctl start nginx
```

---

# 4️⃣ Create Target Group

Go to:

```text id="a7"
EC2 → Target Groups → Create Target Group
```

---

## Configuration

| Field       | Value       |
| ----------- | ----------- |
| Target Type | Instances   |
| Name        | `devops-tg` |
| Protocol    | HTTP        |
| Port        | 80          |
| VPC         | Default VPC |

---

## Health Check Settings

| Setting  | Value |
| -------- | ----- |
| Protocol | HTTP  |
| Path     | `/`   |

Create target group.

---

# 5️⃣ Create Application Load Balancer

Go to:

```text id="a8"
EC2 → Load Balancers → Create Load Balancer
```

Choose:

```text id="a9"
Application Load Balancer
```

---

## Configuration

| Field   | Value           |
| ------- | --------------- |
| Name    | `devops-alb`    |
| Scheme  | Internet-facing |
| IP Type | IPv4            |
| VPC     | Default         |

---

## Availability Zones

Select:

* Minimum 2 public subnets

---

## Security Group

Attach:

```text id="a10"
devops-sg
```

---

## Listener

| Protocol | Port |
| -------- | ---- |
| HTTP     | 80   |

Forward to:

```text id="a11"
devops-tg
```

Create ALB.

---

# 6️⃣ Create Auto Scaling Group

Go to:

```text id="a12"
EC2 → Auto Scaling Groups → Create Auto Scaling Group
```

---

## Configuration

| Field           | Value                    |
| --------------- | ------------------------ |
| Name            | `devops-asg`             |
| Launch Template | `devops-launch-template` |

---

## Network

Choose:

* Default VPC
* Public subnets

---

## Attach to Load Balancer

Select:

```text id="a13"
Attach to existing load balancer
```

Choose:

```text id="a14"
devops-tg
```

---

## Group Size

| Setting          | Value |
| ---------------- | ----- |
| Desired Capacity | 1     |
| Minimum Capacity | 1     |
| Maximum Capacity | 2     |

---

# 7️⃣ Configure Scaling Policy

Choose:

```text id="a15"
Target Tracking Scaling Policy
```

---

## Scaling Configuration

| Setting      | Value                   |
| ------------ | ----------------------- |
| Metric Type  | Average CPU Utilization |
| Target Value | 50                      |

Create ASG.

---

# 8️⃣ Verify EC2 Instance Launch

Go to:

```text id="a16"
EC2 → Instances
```

Verify:

* instance launched automatically
* nginx installed
* instance healthy

---

# 9️⃣ Verify Target Group Health

Go to:

```text id="a17"
EC2 → Target Groups → devops-tg → Targets
```

Wait until:

```text id="a18"
Healthy
```

---

# 🔟 Test ALB DNS

Go to:

```text id="a19"
EC2 → Load Balancers → devops-alb
```

Copy:

```text id="a20"
DNS Name
```

Open browser:

```text id="a21"
http://<ALB-DNS>
```

Expected output:

```text id="a22"
Welcome to nginx!
```

---

# ✅ Validation Checklist

* ✔ Launch Template created
* ✔ User Data configured correctly
* ✔ Auto Scaling Group created
* ✔ Scaling policy configured
* ✔ ALB configured successfully
* ✔ Target group healthy
* ✔ Nginx accessible using ALB DNS

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Target Group Unhealthy

### Root Cause

Nginx not installed or service not running.

---

### Fix

Verify User Data:

```bash id="a23"
#!/bin/bash
yum update -y
amazon-linux-extras install nginx1 -y
systemctl enable nginx
systemctl start nginx
```

---

## 🔴 Issue 2: ALB DNS Not Accessible

### Root Cause

Security group missing HTTP rule.

---

### Fix

Add inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

---

## 🔴 Issue 3: ASG Not Launching Instances

### Root Cause

Incorrect subnet selection.

---

### Fix

Select:

```text id="a24"
Public subnets
```

inside default VPC.

---

## 🔴 Issue 4: Health Checks Failing

### Root Cause

Target group health check path incorrect.

---

### Fix

Set health check path:

```text id="a25"
/
```

---

## 🔴 Issue 5: Scaling Policy Not Triggering

### Root Cause

CPU threshold not configured.

---

### Fix

Use:

```text id="a26"
Average CPU Utilization = 50%
```

---

## 🔴 Issue 6: User Data Script Failed

### Root Cause

Wrong package installation commands.

---

### Fix

Use Amazon Linux 2 compatible commands:

```bash id="a27"
amazon-linux-extras install nginx1 -y
```

---

## 🔴 Issue 7: ALB Created but No Targets Registered

### Root Cause

ASG not attached to target group.

---

### Fix

Attach:

```text id="a28"
devops-tg
```

during ASG configuration.

---

# 🧠 Key Learnings

* ASG improves application availability
* ALB distributes incoming traffic
* Launch Templates standardize EC2 provisioning
* User Data automates software installation
* Target Groups perform health-based routing

---

# 🔍 Real-World Use Cases

This architecture is widely used for:

* Production web applications
* Auto-scaled environments
* Highly available applications
* Microservices hosting
* Enterprise-grade web platforms

---

# 🏁 Final Outcome

✔ Auto Scaling configured successfully
✔ Nginx installed automatically on EC2
✔ Application Load Balancer deployed
✔ Traffic distributed across instances
✔ Application accessible using ALB DNS

---

# 💡 DevOps Insight

> “Auto Scaling and Load Balancing together form the backbone of resilient and highly available cloud-native applications.”
