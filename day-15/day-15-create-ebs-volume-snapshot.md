# 🚀 Day 15 – Create EBS Volume Snapshot

## 🎯 Objective

Create an **Amazon EBS snapshot** from an existing volume to establish a **point-in-time backup**, supporting recovery, migration, and long-term data protection strategies.

---

## 🧠 Why This Matters

EBS snapshots are the foundation of AWS block storage backup and recovery. They enable:

* Point-in-time recovery
* Volume cloning across Availability Zones
* Safe data migration and rollback

Snapshots are **incremental and durable**, making them cost-efficient and reliable for ongoing protection.

---

## 🛠️ Prerequisites

* An existing EBS volume (example: `xfusion-vol`)
* Permissions to create snapshots
* Volume in a stable state (preferably not under heavy write load)

---

## 🖥️ Step-by-Step: Create EBS Snapshot (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the region where the EBS volume exists (example: **us-east-1**)

---

### 2️⃣ Navigate to Volumes

1. Open **AWS Console**
2. Search for **EC2**
3. In the left navigation pane, go to:

   ```
   Elastic Block Store → Volumes
   ```

---

### 3️⃣ Select the Source Volume

* Locate the EBS volume
* Click the **checkbox** next to the volume name

Confirm the correct volume is selected.

---

### 4️⃣ Create Snapshot

* Click **Actions**
* Select **Create snapshot**

---

### 5️⃣ Configure Snapshot Details

* **Description**:

  ```
  xfusion Snapshot
  ```

* **Tags**:

  * Key:

    ```
    Name
    ```
  * Value:

    ```
    xfusion-vol-ss
    ```

Tagging snapshots is critical for identification, automation, and cost tracking.

* Click **Create snapshot**

---

## ⏳ Monitor Snapshot Creation

### 6️⃣ Verify Snapshot Status

* Navigate to:

  ```
  EC2 → Elastic Block Store → Snapshots
  ```

* Locate the snapshot by name tag

* Wait until status shows:

  ```
  Completed
  ```

Do not proceed until the snapshot reaches this state.

---

## ✅ Validation Checklist

* Snapshot name tag is correct
* Snapshot status is **Completed**
* Snapshot is associated with the intended volume

---

## ⚠️ Common Operational Mistakes

* Submitting the task while snapshot is still **Pending**
* Forgetting to tag snapshots
* Assuming snapshots capture live in-memory data

---

## 🧠 Architectural Insight

Snapshots are **crash-consistent**, not application-consistent. For critical workloads, coordinate snapshots with application-level controls to ensure full data integrity.

---

## 📌 Day 15 Summary

Day 15 focused on creating an **EBS volume snapshot**, reinforcing best practices for backup, recovery, and storage lifecycle management in AWS.

* Snapshots as point-in-time, incremental backups
* mportance of waiting for Snapshot = Completed
* Proper tagging for visibility, automation, and cost tracking
* Understanding crash-consistent vs application-consistent backups
---
