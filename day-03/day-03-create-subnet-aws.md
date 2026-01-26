# 🚀 Day 03 – Create Subnet (AWS Networking Fundamentals)

## 🎯 Objective

Design and create an **AWS Subnet** within a VPC, focusing on **network segmentation, availability, and production-grade planning** rather than just basic configuration.

This day introduces **VPC subnetting**, a core skill for any senior DevOps or cloud engineer.

---

## 🧠 Senior DevOps Perspective: Why Subnets Matter

Subnets define:

* **Fault isolation** (Availability Zones)
* **Security boundaries** (public vs private)
* **Scalability limits** (CIDR planning)

Poor subnet design leads to:

* IP exhaustion
* Hard-to-scale architectures
* Complex rework during growth

A senior engineer always plans subnets **before** launching workloads.

---

## 🛠️ Prerequisites

* AWS account access
* Existing VPC (default or custom)
* Basic understanding of CIDR notation

---

## 🖥️ Step-by-Step: Create a Subnet (AWS Console)

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Select required region (example: **us-east-1**)

---

### 2️⃣ Navigate to VPC Dashboard

1. Open **AWS Console**
2. Search for **VPC**
3. Click **VPC**

---

### 3️⃣ Go to Subnets

* Left navigation → **Subnets**
* Click **Create subnet**

---

### 4️⃣ Configure Subnet Details

#### VPC Selection

* Select **Default VPC** (or project-specific VPC)

#### Subnet Configuration

* **Subnet name**:

  ```
  public-subnet-1a
  ```
* **Availability Zone**:

  ```
  us-east-1a
  ```
* **IPv4 CIDR block**:

  ```
  172.31.10.0/24
  ```

> 📌 **Senior Tip**: Always align one subnet per AZ to achieve high availability.

---

### 5️⃣ Create Subnet

* Click **Create subnet**

You should see:

```
Subnet public-subnet-1a created successfully
```

---

## ✅ Validation Checklist

* Subnet exists in correct VPC
* CIDR does not overlap with other subnets
* Subnet mapped to a single AZ
* Status shows **Available**

---

## ⚠️ Common Real-World Mistakes

* Using overly small CIDR blocks
* Placing all subnets in one AZ
* Mixing public and private workloads in same subnet
* Ignoring future growth

---

## 🧠 Architect-Level Insight

> Subnets are not just IP ranges — they are **design decisions** that define reliability, security, and scalability.

Well-planned subnets make future changes predictable and low-risk.

---

## 📌 Day 03 Summary

Day 03 focused on **creating and designing AWS subnets**, reinforcing the importance of network planning as a foundational DevOps responsibility.
