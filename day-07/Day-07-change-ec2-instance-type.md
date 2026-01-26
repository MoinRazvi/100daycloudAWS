# 🚀 Day 07 – Change EC2 Instance Type (Compute Right-Sizing)

## 🎯 Objective

Modify the **instance type of an existing EC2 instance** to align compute capacity with workload needs, reinforcing **cost awareness, operational safety, and controlled change management**.

---

## 🧠 Why This Matters

Changing an EC2 instance type is a common operational task during:

* Performance tuning
* Cost optimization
* Environment right-sizing (dev/test vs prod)

This operation requires a **stop–modify–start lifecycle**, and understanding this flow is critical to avoiding unplanned downtime.

---

## 🛠️ Prerequisites

* An existing EC2 instance
* Permissions to stop and modify instances
* Awareness of potential downtime during the change

---

## 🖥️ Step-by-Step: Change EC2 Instance Type (AWS Console)

### 1️⃣ Select the Correct Region

* AWS Console → top-right
* Choose the region where the instance is running (example: **us-east-1**)

---

### 2️⃣ Navigate to EC2 Instances

1. Open **AWS Console**
2. Search for **EC2**
3. Click **Instances**

---

### 3️⃣ Select the Target Instance

* Identify the EC2 instance to be modified
* Click the **checkbox** next to the instance name

Ensure the correct instance is selected before proceeding.

---

### 4️⃣ Stop the Instance

* From the top menu, click **Instance state**
* Select **Stop instance**
* Confirm the action

Wait until:

```
Instance state: Stopped
```

> 📌 Instance type changes are not allowed while the instance is running.

---

### 5️⃣ Change Instance Type

* With the instance still selected and **stopped**:

* Click **Actions**

* Navigate to:

  ```
  Instance settings → Change instance type
  ```

* Choose the new instance type (example: `t2.nano`)

* Click **Apply**

---

### 6️⃣ Start the Instance

* Click **Instance state**
* Select **Start instance**

Wait until:

```
Instance state: Running
```

---

## ✅ Validation Checklist

* Instance type reflects the new selection
* Instance state is **Running**
* No configuration drift introduced

---

## ⚠️ Common Operational Mistakes

* Attempting to change instance type while running
* Forgetting to restart the instance
* Changing instance type without evaluating memory/CPU needs
* Performing the change during peak traffic hours

---

## 🧠 Architectural Insight

Right-sizing compute is an **ongoing process**, not a one-time decision. Infrastructure should evolve with workload demand to remain efficient and resilient.

---

## 📌 Day 07 Summary

Day 07 focused on **safely modifying EC2 compute capacity**, reinforcing best practices around controlled changes, downtime awareness, and cost-conscious infrastructure management.
* Instance resizing as a controlled operational change
* Stop → modify → start lifecycle awareness
* Downtime and timing considerations
* Cost and performance alignment through right-sizing
---
