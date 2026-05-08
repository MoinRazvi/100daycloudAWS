
# 🚀 Day 32 – RDS Snapshot and Restore (Backup & Recovery Validation)

## 📘 Topic

Database Backup and Recovery using Amazon RDS Snapshots

---

## 🎯 Objective

Create a **snapshot backup** of an existing RDS instance and restore it into a new instance to validate:

* Backup reliability
* Data recovery capability
* Infrastructure readiness for updates

This simulates a **real-world pre-production validation workflow**.

---

## 🏗️ Architecture Overview

```id="arch32"
Original RDS (nautilus-rds)
        ↓
   Snapshot (nautilus-snapshot)
        ↓
Restored RDS (nautilus-snapshot-restore)
```

---

## 🧱 Key Requirements

* Source RDS: `nautilus-rds`
* Snapshot Name: `nautilus-snapshot`
* Restored DB: `nautilus-snapshot-restore`
* Instance Class: `db.t3.micro`
* Final State: **Available**

---

## 🛠️ Implementation Steps

---

### 1️⃣ Ensure Source RDS is Available

Go to:

```id="step1"
RDS → Databases → nautilus-rds
```

Verify:

```id="step1check"
Status = Available
```

---

### 2️⃣ Take Snapshot

Navigate to:

```id="step2"
RDS → Databases → nautilus-rds → Actions → Take snapshot
```

Provide:

```id="step2name"
Snapshot identifier: nautilus-snapshot
```

Wait until:

```id="step2status"
Snapshot status = Available
```

---

### 3️⃣ Restore Snapshot

Go to:

```id="step3"
RDS → Snapshots → nautilus-snapshot → Restore snapshot
```

---

### 4️⃣ Configure Restored Instance

Set:

```id="step4"
DB instance identifier: nautilus-snapshot-restore
DB instance class: db.t3.micro
```

---

### 5️⃣ Connectivity & Settings

Keep:

```id="step5"
Same VPC / Subnet Group
Private access (recommended)
Security Group same or controlled
```

---

### 6️⃣ Launch Restore

Click:

```id="step6"
Restore DB instance
```

---

### 7️⃣ Wait for Completion

Monitor:

```id="step7"
Status → Available
```

---

## 🧪 Validation

* ✔ Snapshot successfully created
* ✔ Snapshot status = Available
* ✔ Restored instance created
* ✔ Instance class = db.t3.micro
* ✔ New RDS status = Available

---

# 🚨 Real-World Importance

## 🔴 Why This Matters

* Protects against **data loss**
* Enables **safe testing before production changes**
* Supports **disaster recovery strategies**

---

# ⚠️ Common Mistakes

### ❌ Taking snapshot while DB not available

* Causes failure or delay

---

### ❌ Not waiting for snapshot completion

* Restore option may not appear

---

### ❌ Wrong instance class after restore

* Breaks free tier requirement

---

### ❌ Changing VPC/Subnet incorrectly

* Causes connectivity issues

---

### ❌ Assuming snapshot includes everything

> Snapshot includes:

* Data
* Schema
* Configuration (mostly)

But:

* Security groups & networking still matter

---

# 🧠 Key Learnings

* RDS snapshot = **point-in-time backup**
* Restore = **full database clone**
* Snapshot → restore is core **DR strategy**
* Always validate backups before production changes

---

# 🔍 Advanced Insight

> Snapshots are stored in S3 (managed by AWS), but you don’t interact with S3 directly.

---

# 🏁 Final Outcome

✔ Snapshot created successfully
✔ New RDS restored from snapshot
✔ Instance verified in Available state
✔ Backup and restore workflow validated

---
