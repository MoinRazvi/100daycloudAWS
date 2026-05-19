# 🚀 Day 45 – Private EC2 Internet Access Using NAT Gateway

## 📘 Topic

Enabling internet access for a private EC2 instance using AWS NAT Gateway architecture.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create a public subnet
* Configure Internet Gateway
* Create NAT Gateway
* Configure route tables
* Enable internet access for private EC2
* Allow EC2 to upload files to S3

This task demonstrates:

* Private subnet internet access
* NAT Gateway architecture
* VPC routing
* Public vs Private subnet design
* Secure outbound internet connectivity

---

# 🏗️ AWS Architecture Diagram

```text id="nat45"
                    ┌──────────────────────────────┐
                    │         AWS Cloud            │
                    └─────────────┬────────────────┘
                                  │
                             Internet
                                  │
                     ┌────────────▼────────────┐
                     │    Internet Gateway     │
                     │         (IGW)           │
                     └────────────┬────────────┘
                                  │
          ┌───────────────────────┴────────────────────────┐
          │                 nautilus-priv-vpc              │
          │                                                 │
          │   ┌──────────────────────────────┐              │
          │   │      Public Subnet           │              │
          │   │   nautilus-pub-subnet        │              │
          │   │                              │              │
          │   │   ┌────────────────────┐     │              │
          │   │   │    NAT Gateway     │     │              │
          │   │   │   nautilus-natgw   │     │              │
          │   │   └─────────┬──────────┘     │              │
          │   └─────────────┼────────────────┘              │
          │                 │                               │
          │      Route: 0.0.0.0/0                           │
          │                 │                               │
          │   ┌─────────────▼────────────────┐              │
          │   │       Private Subnet         │              │
          │   │    nautilus-priv-subnet      │              │
          │   │                               │              │
          │   │   ┌────────────────────┐      │              │
          │   │   │  nautilus-priv-ec2 │      │              │
          │   │   │     Private EC2    │      │              │
          │   │   └────────────────────┘      │              │
          │   └───────────────────────────────┘              │
          └──────────────────────────────────────────────────┘
                                  │
                                  ▼
                     Amazon S3 Bucket
               nautilus-nat-264638244
```

---

# 🧱 Components Used

| Component          | Name                     |
| ------------------ | ------------------------ |
| VPC                | `nautilus-priv-vpc`      |
| Private Subnet     | `nautilus-priv-subnet`   |
| Public Subnet      | `nautilus-pub-subnet`    |
| NAT Gateway        | `nautilus-natgw`         |
| Public Route Table | `nautilus-pub-rt`        |
| EC2 Instance       | `nautilus-priv-ec2`      |
| S3 Bucket          | `nautilus-nat-264638244` |

---

# 🔄 Traffic Flow

```text id="natflow"
Private EC2
    ↓
Private Route Table
    ↓
NAT Gateway
    ↓
Internet Gateway
    ↓
Internet / AWS Services
    ↓
Amazon S3 Bucket
```

---

# 🛠️ High-Level Setup Steps

---

## 1️⃣ Create Public Subnet

Create:

```text id="nat1"
nautilus-pub-subnet
```

inside:

```text id="nat2"
nautilus-priv-vpc
```

---

## 2️⃣ Create Internet Gateway

Create and attach:

```text id="nat3"
Internet Gateway
```

to:

```text id="nat4"
nautilus-priv-vpc
```

---

## 3️⃣ Create Public Route Table

Create:

```text id="nat5"
nautilus-pub-rt
```

Add route:

| Destination | Target           |
| ----------- | ---------------- |
| `0.0.0.0/0` | Internet Gateway |

Associate with:

```text id="nat6"
nautilus-pub-subnet
```

---

## 4️⃣ Allocate Elastic IP

Allocate:

```text id="nat7"
Elastic IP
```

for NAT Gateway.

---

## 5️⃣ Create NAT Gateway

Create:

```text id="nat8"
nautilus-natgw
```

inside:

```text id="nat9"
nautilus-pub-subnet
```

Attach:

```text id="nat10"
Elastic IP
```

---

## 6️⃣ Update Private Route Table

Modify route table associated with:

```text id="nat11"
nautilus-priv-subnet
```

Add:

| Destination | Target      |
| ----------- | ----------- |
| `0.0.0.0/0` | NAT Gateway |

---

## 7️⃣ Verify Internet Access

The private EC2 cron job uploads:

```text id="nat12"
test file
```

to:

```text id="nat13"
nautilus-nat-264638244
```

Wait:

```text id="nat14"
2–3 minutes
```

Verify file appears in S3 bucket.

---

# ✅ Validation Checklist

* ✔ Public subnet created
* ✔ Internet Gateway attached
* ✔ Public route table configured
* ✔ NAT Gateway available
* ✔ Private route table updated
* ✔ Private EC2 gained internet access
* ✔ Test file uploaded to S3 successfully

---

# 🚨 Common Troubleshooting Issues

---

## 🔴 NAT Gateway Stuck in Failed State

### Root Cause

No Elastic IP attached.

---

### Fix

Allocate and attach:

```text id="nat15"
Elastic IP
```

---

## 🔴 Private EC2 Still Has No Internet

### Root Cause

Private route table missing:

```text id="nat16"
0.0.0.0/0 → NAT Gateway
```

---

## 🔴 NAT Gateway Not Reachable

### Root Cause

Public subnet route table missing IGW route.

---

### Fix

Add:

```text id="nat17"
0.0.0.0/0 → Internet Gateway
```

---

## 🔴 S3 File Not Uploading

### Root Cause

Cron job delay.

---

### Fix

Wait:

```text id="nat18"
2–3 minutes
```

---

## 🔴 NAT Gateway Created in Wrong Subnet

### Root Cause

NAT Gateway must be in:

```text id="nat19"
Public Subnet
```

NOT private subnet.

---

# 🧠 Key Learnings

* Private EC2 instances cannot directly access internet
* NAT Gateway enables secure outbound connectivity
* Internet Gateway is required for public subnet access
* Route tables control traffic direction
* NAT Gateway improves security by preventing inbound internet access

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Private application servers
* Backend APIs
* Secure enterprise workloads
* Internal Kubernetes worker nodes
* Private patch/update servers

---

# 🏁 Final Outcome

✔ NAT Gateway configured successfully
✔ Private EC2 gained outbound internet access
✔ S3 upload validated successfully
✔ Secure AWS private subnet architecture implemented

---

# 💡 DevOps Insight

> “NAT Gateway allows private workloads to securely access the internet without exposing them directly to inbound public traffic.”
#
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/5e5ee343-d05c-45ac-ae48-ac7ed9a60606" />


