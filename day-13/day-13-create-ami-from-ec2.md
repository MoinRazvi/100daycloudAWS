# 🚀 Day 13 – Create AMI from EC2 Instance

## 🎯 Objective

Create an **Amazon Machine Image (AMI)** from an existing EC2 instance to enable **repeatable deployments, fast recovery, and consistent infrastructure provisioning**.

---

## 🧠 Why This Matters

AMIs capture the **complete state** of an EC2 instance, including:

* Operating system
* Installed packages and configurations
* Attached root volume snapshot

Creating AMIs is fundamental for:

* Backup and disaster recovery
* Golden image strategies
* Scaling environments with consistent baselines

---

## 🛠️ Prerequisites

* An existing EC2 instance (example: `datacenter-ec2` or `nautilus-ec2`)
* Permissions to create AMIs and snapshots
* Instance in a stable state (no critical updates running)

---

## 🖥️ Step-by-Step: Create AMI (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the region where the EC2 instance exists (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2 Instances

1. Open **AWS Console**
2. Search for **EC2**
3. Click **Instances**

---

### 3️⃣ Select the EC2 Instance

* Locate the source EC2 instance
* Click the **checkbox** next to the instance name

Confirm the correct instance is selected before proceeding.

---

### 4️⃣ Create Image (AMI)

* From the top menu, click **Actions**
* Navigate to:

  ```
  Image and templates → Create image
  ```

---

### 5️⃣ Configure AMI Details

* **Image name**:

  ```
  datacenter-ec2-ami
  ```

* **Image description** (optional):

  ```
  AMI created from datacenter-ec2
  ```

* Leave all other options as default

> 📌 AWS will automatically create snapshots of the required volumes.

* Click **Create image**

---

## ⏳ Monitor AMI Creation

### 6️⃣ Track AMI Status

* Go to:

  ```
  EC2 → Images → AMIs
  ```

* Locate the AMI by name

* Wait until status shows:

  ```
  Available
  ```

AMI creation may take several minutes depending on volume size.

---

## ✅ Validation Checklist

* AMI name matches expected value
* AMI status is **Available**
* AMI is visible under **Owned by me**

---

## ⚠️ Common Operational Mistakes

* Submitting task while AMI is still **Pending**
* Creating AMI during active application writes
* Confusing snapshots with AMIs

---

## 🧠 Architectural Insight

AMIs are **immutable artifacts**. Treat them as versioned assets that can be promoted across environments, ensuring consistency and traceability.

---

## 📌 Day 13 Summary

Day 13 focused on creating an **AMI from an EC2 instance**, reinforcing best practices around image-based infrastructure, recovery readiness, and repeatable deployments.

* AMIs as immutable infrastructure artifacts
* Difference between snapshots vs AMIs
* Importance of waiting for AMI = Available
* Using AMIs for backup, recovery, and consistency
---
