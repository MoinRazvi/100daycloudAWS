# 🚀 Day 48 – Secure Log Aggregation Across VPCs Using VPC Peering + S3

## 📘 Topic

Building a secure cross-VPC log aggregation pipeline using:

* EC2
* VPC Peering
* IAM Roles
* S3
* Cron Jobs

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Transfer logs from a private EC2 instance
* Move logs securely across VPCs
* Upload logs to S3 automatically
* Use VPC Peering for secure communication
* Use IAM roles for secure S3 access

This task demonstrates:

* Hybrid private/public VPC design
* Secure VPC peering
* Automated log shipping
* IAM role-based S3 access
* Cross-VPC communication

---

# 🏗️ AWS Architecture Diagram

```text id="d48a1"
                    ┌─────────────────────────────┐
                    │         AWS Cloud           │
                    └──────────────┬──────────────┘
                                   │
                              Internet
                                   │
                    ┌──────────────▼──────────────┐
                    │      Internet Gateway       │
                    └──────────────┬──────────────┘
                                   │
         ┌─────────────────────────▼─────────────────────────┐
         │               datacenter-pub-vpc                 │
         │                                                   │
         │  ┌─────────────────────────────────────────────┐  │
         │  │        datacenter-pub-subnet               │  │
         │  │                                             │  │
         │  │   ┌─────────────────────────────────────┐   │  │
         │  │   │       datacenter-pub-ec2            │   │  │
         │  │   │                                     │   │  │
         │  │   │ Receives boots.log from private EC2 │   │  │
         │  │   │ Uploads logs to S3 using IAM Role   │   │  │
         │  │   └─────────────────────────────────────┘   │  │
         │  └─────────────────────────────────────────────┘  │
         └──────────────────────┬────────────────────────────┘
                                │
                    VPC Peering Connection
                     datacenter-vpc-peering
                                │
         ┌──────────────────────▼────────────────────────────┐
         │              datacenter-priv-vpc                 │
         │                                                   │
         │  ┌─────────────────────────────────────────────┐  │
         │  │       datacenter-priv-subnet               │  │
         │  │                                             │  │
         │  │   ┌─────────────────────────────────────┐   │  │
         │  │   │      datacenter-priv-ec2            │   │  │
         │  │   │                                     │   │  │
         │  │   │ Cron Job Sends boots.log via SCP    │   │  │
         │  │   └─────────────────────────────────────┘   │  │
         │  └─────────────────────────────────────────────┘  │
         └───────────────────────────────────────────────────┘
                                   │
                                   ▼
                    ┌────────────────────────┐
                    │     Private S3 Bucket  │
                    │ datacenter-s3-logs-    │
                    │        21006           │
                    └────────────────────────┘

S3 Object Path:
datacenter-priv-vpc/boot/boots.log
```

---

# 🧱 AWS Components Used

| Component           | Name                       |
| ------------------- | -------------------------- |
| Private VPC         | `datacenter-priv-vpc`      |
| Public VPC          | `datacenter-pub-vpc`       |
| Private Subnet      | `datacenter-priv-subnet`   |
| Public Subnet       | `datacenter-pub-subnet`    |
| Public Route Table  | `datacenter-pub-rt`        |
| Private Route Table | `datacenter-priv-rt`       |
| Public EC2          | `datacenter-pub-ec2`       |
| Private EC2         | `datacenter-priv-ec2`      |
| IAM Role            | `datacenter-s3-role`       |
| S3 Bucket           | `datacenter-s3-logs-21006` |
| VPC Peering         | `datacenter-vpc-peering`   |

---
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/db6516d1-05ff-43b9-ac0a-f87df2534e07" />


# 🔄 Workflow

```text id="d48a2"
Private EC2
    ↓
Cron Job (scp/rsync)
    ↓
Public EC2
    ↓
IAM Role Authentication
    ↓
AWS S3 Bucket
    ↓
Store Logs:
datacenter-priv-vpc/boot/boots.log
```

---

# 🛠️ High-Level Setup Steps

---

# 1️⃣ Create Public VPC

Create:

```text id="d48a3"
datacenter-pub-vpc
```

---

# 2️⃣ Create Public Subnet

Create:

```text id="d48a4"
datacenter-pub-subnet
```

inside:

```text id="d48a5"
datacenter-pub-vpc
```

---

# 3️⃣ Create Public Route Table

Create:

```text id="d48a6"
datacenter-pub-rt
```

Associate with:

```text id="d48a7"
datacenter-pub-subnet
```

---

# 4️⃣ Create Internet Gateway

Create and attach:

```text id="d48a8"
Internet Gateway
```

to:

```text id="d48a9"
datacenter-pub-vpc
```

Add route:

| Destination | Target           |
| ----------- | ---------------- |
| `0.0.0.0/0` | Internet Gateway |

---

# 5️⃣ Launch Public EC2

Launch:

```text id="d48a10"
datacenter-pub-ec2
```

Use:

* Ubuntu AMI
* Same key pair:

  ```text
  datacenter-key.pem
  ```

---

# 6️⃣ Create IAM Role

Create:

```text id="d48a11"
datacenter-s3-role
```

Attach policy:

```json id="d48a12"
s3:PutObject
```

for:

```text
datacenter-s3-logs-21006
```

Attach role to:

```text id="d48a13"
datacenter-pub-ec2
```

---

# 7️⃣ Create Private S3 Bucket

Create:

```text id="d48a14"
datacenter-s3-logs-21006
```

Keep:

```text id="d48a15"
Private Access
```

enabled.

---

# 8️⃣ Create VPC Peering

Create:

```text id="d48a16"
datacenter-vpc-peering
```

Between:

* datacenter-priv-vpc
* datacenter-pub-vpc

Accept peering request.

---

# 9️⃣ Update Route Tables

---

## Private Route Table

Add route:

| Destination     | Target      |
| --------------- | ----------- |
| Public VPC CIDR | VPC Peering |

---

## Public Route Table

Add route:

| Destination      | Target      |
| ---------------- | ----------- |
| Private VPC CIDR | VPC Peering |

---

# 🔟 Configure SSH Access

Use:

```text id="d48a17"
/root/.ssh/datacenter-key.pem
```

to establish passwordless SSH between:

* private EC2
* public EC2

---

# 1️⃣1️⃣ Configure Cron Job on Private EC2

Transfer:

```text id="d48a18"
/var/log/boots.log
```

to public EC2 using:

* scp
  or
* rsync

---

# 1️⃣2️⃣ Configure Cron Job on Public EC2

Upload logs to S3:

```text id="d48a19"
s3://datacenter-s3-logs-21006/datacenter-priv-vpc/boot/boots.log
```

using:

```text id="d48a20"
aws s3 cp
```

---

# ✅ Final Validation

Verify object exists:

```text id="d48a21"
datacenter-priv-vpc/boot/boots.log
```

inside:

```text id="d48a22"
datacenter-s3-logs-21006
```

---

# 🚨 Common Troubleshooting Issues

---

# 🔴 VPC Peering Not Working

## Root Cause

Missing routes in route tables.

---

## Fix

Add reciprocal CIDR routes through:

```text id="d48a23"
datacenter-vpc-peering
```

---

# 🔴 SCP Connection Refused

## Root Cause

Security group missing SSH rule.

---

## Fix

Allow:

| Type | Port |
| ---- | ---- |
| SSH  | 22   |

between VPC CIDRs.

---

# 🔴 S3 Upload Access Denied

## Root Cause

IAM role missing:

```text id="d48a24"
s3:PutObject
```

permission.

---

# 🔴 Cron Job Not Running

## Root Cause

Incorrect cron syntax or permissions.

---

## Fix

Verify:

```bash
crontab -l
```

---

# 🔴 Route Table Associated with Wrong Subnet

## Root Cause

Public subnet not associated with:

```text id="d48a25"
datacenter-pub-rt
```

---

# 🧠 Key Learnings

* VPC Peering enables secure cross-VPC communication
* Private workloads can securely export logs
* IAM roles eliminate hardcoded AWS credentials
* Cron jobs automate operational tasks
* S3 provides centralized durable log storage

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Centralized logging
* Compliance log archival
* Cross-network monitoring
* Secure enterprise log aggregation
* Multi-VPC infrastructure operations

---

# 🏁 Final Outcome

✔ Public and private VPCs configured
✔ Secure VPC peering established
✔ Logs transferred across VPCs
✔ Automated S3 uploads configured
✔ Secure centralized log aggregation implemented

---

# 💡 DevOps Insight

> “Secure log aggregation pipelines are foundational for observability, compliance, and incident response in cloud-native environments.”
> 
