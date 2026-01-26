# 🚀 Day 05 – Create GP3 EBS Volume (Elastic Block Store)

## 🎯 Objective

Create an **Amazon EBS gp3 volume** to provide persistent block storage for EC2 workloads, emphasizing **correct sizing, availability zone alignment, and production-ready defaults**.

---

## 🧠 Why GP3 Matters

Amazon EBS **gp3** volumes decouple performance from size, providing predictable baseline performance at lower cost. They are the default choice for most general-purpose workloads and a foundational building block for scalable architectures.

Key characteristics:

* Cost-efficient general-purpose storage
* Independent scaling of IOPS and throughput
* Designed for a wide range of workloads (apps, databases, logs)

---

## 🛠️ Prerequisites

* AWS account access
* EC2 service familiarity
* Understanding that EBS volumes are **Availability Zone–scoped**

---

## 🖥️ Step-by-Step: Create a GP3 EBS Volume (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the required region (example: **us-east-1**)

---

### 2️⃣ Navigate to Volumes

1. Open **AWS Console**
2. Search for **EC2**
3. In the left navigation pane, go to:

   ```
   Elastic Block Store → Volumes
   ```

---

### 3️⃣ Create a New Volume

* Click **Create volume**

---

### 4️⃣ Configure Volume Details

#### Volume Configuration

* **Volume type**:

  ```
  gp3
  ```
* **Size (GiB)**:

  ```
  2
  ```
* **Availability Zone**:

  ```
  Select the same AZ as the target EC2 instance
  ```

> 📌 Volumes can only be attached to instances within the **same AZ**.

---

### 5️⃣ Add Name Tag

Under **Tags**:

* **Key**:

  ```
  Name
  ```
* **Value**:

  ```
  xfusion-volume
  ```

Clear tagging is essential for operations, billing, and automation.

---

### 6️⃣ Create the Volume

* Click **Create volume**

You should see:

```
Volume xfusion-volume created successfully
```

---

## ✅ Validation Checklist

* Volume type is **gp3**
* Size is correctly set
* Volume is in the correct Availability Zone
* State shows **Available**
* Name tag is applied

---

## ⚠️ Common Operational Mistakes

* Creating the volume in a different AZ than the EC2 instance
* Forgetting to tag volumes
* Overprovisioning size instead of scaling performance
* Using gp2 by habit instead of gp3

---

## 🧠 Architectural Insight

EBS volumes are **independent lifecycle resources**. Designing storage separately from compute enables:

* Safer instance replacement
* Easier backups via snapshots
* Flexible scaling and recovery

---

## 📌 Day 05 Summary

Day 05 focused on creating a **gp3 EBS volume** with correct configuration and tagging, reinforcing best practices for persistent storage management in AWS.
* What Day 05 emphasizes (without calling it out explicitly)
* Why gp3 is the default choice today
* AZ alignment (a very common real-world failure point)
* Importance of tagging for ops, cost, and automation
* Storage as an independent lifecycle resource, not tied to EC2

---
