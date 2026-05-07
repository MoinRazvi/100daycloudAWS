# 🚀 Day 31 – Provisioning a Private RDS (MySQL) Instance

## 📘 Topic

Private Managed Database Deployment using Amazon RDS (MySQL Free Tier)

---

## 🎯 Objective

Provision a **private Amazon RDS instance** using MySQL to support application development, ensuring:

* Cost optimization using **Free Tier**
* Secure deployment inside a **private network**
* Scalability via **storage autoscaling**
* Availability and readiness for application use

---

## 🏗️ Architecture Overview

```id="v9g2xt"
Application (EC2 / App Layer)
        ↓
Private Subnet
        ↓
RDS (MySQL - Private)
```

> RDS is not publicly accessible and can only be accessed from within the VPC.

---

## 🧱 Key Requirements

* DB Instance Identifier: `datacenter-rds`
* Engine: MySQL
* Version: 8.4.x
* Instance Type: `db.t3.micro`
* Template: Free tier
* Deployment: Private
* Storage Autoscaling: Enabled (Max: 50GB)

---

## 🛠️ Implementation Steps

---

### 1️⃣ Create RDS Instance

Go to:

```
AWS Console → RDS → Create database
```

---

### 2️⃣ Choose Creation Method

Select:

```id="o9kg3u"
Standard create (Full configuration)
```

---

### 3️⃣ Engine Configuration

```id="t4qf4z"
Engine type: MySQL
Version: 8.4.x
```

---

### 4️⃣ Templates

Select:

```id="1y91y2"
Free tier
```

---

### 5️⃣ Settings

```id="u1r0mw"
DB instance identifier: datacenter-rds
Master username: admin
Password: (set manually)
```

---

### 6️⃣ Instance Configuration

```id="1x7i5g"
DB instance class: db.t3.micro
```

---

### 7️⃣ Storage

```id="j3gj4b"
Enable storage autoscaling: YES
Maximum storage threshold: 50 GB
```

---

### 8️⃣ Connectivity (Critical)

```id="r5j7xz"
VPC: devops-priv-vpc (or your working VPC)
Public access: NO
```

👉 This ensures RDS is **private**

---

### 9️⃣ DB Subnet Group

* Select existing OR create new
* Must include:

```id="5n3m6r"
Private subnets only
```

---

### 🔟 Security Group

Allow access from application layer:

```id="6zj4kw"
Type: MySQL/Aurora
Port: 3306
Source: VPC CIDR or EC2 SG
```

---

### 1️⃣1️⃣ Additional Configuration

Keep default unless required.

---

### 1️⃣2️⃣ Create Database

Click:

```id="p4ch0d"
Create database
```

---

## 🧪 Validation

Wait until:

```id="n6h7c2"
Status: Available
```

---

## 🔍 Verification Checks

* ✔ RDS instance is **Available**
* ✔ Public access = **No**
* ✔ Located in **private subnet**
* ✔ Storage autoscaling enabled
* ✔ Engine version = MySQL 8.4.x

---

# 🚨 Real-World Considerations

## 🔴 Common Mistakes

### 1. Public Access Enabled ❌

* Makes DB exposed to internet
* Violates security best practices

---

### 2. Wrong Subnet Group ❌

* Includes public subnet → breaks “private” requirement

---

### 3. Security Group Too Open ❌

```id="badsg"
0.0.0.0/0 on port 3306
```

---

### 4. Not Using Free Tier ❌

* Leads to unnecessary cost

---

### 5. Forgetting Storage Autoscaling ❌

* Can cause DB outages later

---

# 🧠 Key Learnings

* RDS is a **managed database service**
* Private RDS improves **security posture**
* Subnet group determines **network placement**
* Security Groups control **DB access**
* Storage autoscaling ensures **scalability**

---

# 🏁 Final Outcome

✔ Private MySQL RDS instance created
✔ Free tier optimized
✔ Secure and scalable configuration
✔ Ready for application integration

---
