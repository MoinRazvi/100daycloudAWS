# 🚀 Day 12 – Attach EBS Volume to EC2 Instance

## 🎯 Objective

Attach an existing **Amazon EBS volume** to an EC2 instance to provide **persistent block storage**, reinforcing correct handling of storage lifecycles and Availability Zone constraints.

---

## 🧠 Why This Matters

EBS volumes are independent storage resources. Attaching them correctly enables:

* Data persistence beyond instance lifecycle
* Flexible storage scaling
* Safe recovery and migration workflows

Correct attachment is a prerequisite for formatting, mounting, and application use.

---

## 🛠️ Prerequisites

* An existing EC2 instance (example: `nautilus-ec2`)
* An existing EBS volume (example: `nautilus-volume`)
* EC2 instance and EBS volume must be in the **same Availability Zone**

---

## 🖥️ Step-by-Step: Attach EBS Volume (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the region where the EC2 instance and volume exist (example: **us-east-1**)

---

### 2️⃣ Navigate to Volumes

1. Open **AWS Console**
2. Search for **EC2**
3. In the left navigation pane, go to:

   ```
   Elastic Block Store → Volumes
   ```

---

### 3️⃣ Select the EBS Volume

* Locate the volume to be attached
* Click the **checkbox** next to the volume name

Ensure the volume state is:

```
Available
```

---

### 4️⃣ Attach the Volume

* Click **Actions**
* Select **Attach volume**

Configure attachment:

* **Instance**: `nautilus-ec2`

* **Device name**:

  ```
  /dev/sdb
  ```

* Click **Attach**

---

## ⏳ Attachment Verification

Wait until the volume state updates to:

```
In-use
```

This confirms the volume is successfully attached to the instance.

---

## ✅ Validation Checklist

* Volume state is **In-use**
* Volume is attached to the correct EC2 instance
* Device name is set correctly

---

## ⚠️ Common Operational Mistakes

* Attaching volume to an instance in a different AZ
* Using an incorrect or conflicting device name
* Assuming attachment automatically mounts the volume

---

## 🧠 Architectural Insight

EBS attachment is a **control-plane operation**. Attaching storage does not expose it to the OS until it is formatted and mounted, which must be done explicitly.

---

## 📌 Day 12 Summary

Day 12 focused on attaching an **EBS volume to an EC2 instance**, reinforcing proper storage lifecycle management and preparing the instance for persistent data usage.

* EBS as an independent storage lifecycle
* Strict Availability Zone alignment
* Correct device naming during attachment
* Understanding that attach ≠ mount (OS-level work comes next)
---
