# 🚀 Day 25 – Setting Up an EC2 Instance and CloudWatch Alarm

## 🎯 Objective

Launch an **EC2 instance** and configure a **CloudWatch alarm** to monitor CPU utilization, triggering notifications via **Amazon SNS** when thresholds are breached.

This day focuses on **observability and proactive monitoring**, which are essential for maintaining reliable cloud workloads.

---

## 🧠 Why This Matters

Without monitoring, infrastructure failures are detected **after users are impacted**. CloudWatch alarms enable:

* Early detection of performance issues
* Automated notifications
* Data-driven scaling and troubleshooting

Monitoring is a foundational pillar of production readiness.

---

## 🛠️ Prerequisites

* AWS account with EC2, CloudWatch, and SNS permissions
* Existing SNS topic (example: `nautilus-sns-topic`)
* Basic understanding of metrics and thresholds

---

## 🖥️ Architecture Diagram (Logical View)

```
+-------------------+
|   Amazon SNS      |
| nautilus-sns-topic|
+---------▲---------+
          |
          | Alarm Notification
          |
+---------+---------+
| Amazon CloudWatch |
|  CPU Utilization  |
+---------▲---------+
          |
          | Metric Data
          |
+---------+---------+
|   EC2 Instance    |
|   nautilus-ec2    |
+-------------------+
```

---

## 🖥️ Step-by-Step: Launch EC2 Instance

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Choose the required region (example: **us-east-1**)

---

### 2️⃣ Open EC2 Service

* Search **EC2** → Click **EC2**

---

### 3️⃣ Launch Instance

Configure:

* **Name**:

  ```
  nautilus-ec2
  ```
* **AMI**: Ubuntu LTS
* **Instance type**:

  ```
  t2.micro
  ```

Network & security:

* Select appropriate VPC and subnet
* Attach security group allowing required access

Launch the instance and wait until state is **Running**.

---

## 📊 Step-by-Step: Create CloudWatch Alarm

### 4️⃣ Navigate to CloudWatch

* AWS Console → Search **CloudWatch** → Click **CloudWatch**

---

### 5️⃣ Create Alarm

* Left menu → **Alarms** → **Create alarm**

---

### 6️⃣ Select Metric

Navigate:

```
EC2 → Per-Instance Metrics → CPUUtilization
```

* Select the metric for `nautilus-ec2`
* Click **Select metric**

---

### 7️⃣ Configure Alarm Conditions

Set:

* **Statistic**: Average
* **Period**: 5 minutes
* **Threshold type**: Static
* **Condition**:

  ```
  CPUUtilization ≥ 90
  ```

---

### 8️⃣ Configure Notifications

* **Alarm state trigger**: In alarm
* **Send notification to**:

  ```
  nautilus-sns-topic
  ```

---

### 9️⃣ Name and Create Alarm

* **Alarm name**:

  ```
  nautilus-alarm
  ```

* Click **Create alarm**

---

## ✅ Validation Checklist

* EC2 instance is **Running**
* CloudWatch alarm state is **OK** initially
* SNS topic is correctly attached
* Alarm transitions to **ALARM** if CPU exceeds threshold

---

## ⚠️ Common Operational Mistakes

* Selecting the wrong EC2 metric
* Using incorrect evaluation periods
* Forgetting to attach SNS notifications

---

## 🧠 Architectural Insight

Monitoring should be designed around **actionable signals**, not noise. Thresholds must balance sensitivity with stability to avoid alert fatigue.

---

## 📌 Day 25 Summary

Day 25 focused on establishing **baseline monitoring** by combining EC2 metrics, CloudWatch alarms, and SNS notifications to enable proactive infrastructure management.

* Monitoring as a first-class responsibility
* Correct CloudWatch metric selection
* Sensible alarm thresholds and periods
* Using SNS for decoupled notifications
* Avoiding alert fatigue through design
  
---
