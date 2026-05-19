# 🚀 Day 43 – Private Amazon EKS Cluster Deployment

## 📘 Topic

Deploying a private Amazon EKS (Elastic Kubernetes Service) cluster using the latest Kubernetes version with high availability across multiple Availability Zones.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create an Amazon EKS cluster
* Use the latest stable Kubernetes version
* Configure private endpoint access only
* Use default VPC across AZs:

  * us-east-1a
  * us-east-1b
  * us-east-1c
* Use existing IAM role:

  ```text
  eksClusterRole
  ```
* Disable:

  ```text
  EKS Auto Mode
  ```

This task demonstrates:

* Managed Kubernetes on AWS
* Private Kubernetes control plane
* High availability EKS architecture
* Multi-AZ Kubernetes deployment
* Secure cluster endpoint configuration

---

# 🏗️ Architecture Overview

```text id="e1"
Developer / kubectl
        ↓
Private EKS Endpoint
        ↓
Amazon EKS Cluster
(nautilus-eks)
        ↓
Control Plane
        ↓
Default VPC
        ↓
Subnets across:
AZ-a | AZ-b | AZ-c
```

---

# 🧱 Requirements

| Component          | Value            |
| ------------------ | ---------------- |
| Cluster Name       | `nautilus-eks`   |
| IAM Role           | `eksClusterRole` |
| Endpoint Access    | Private          |
| Networking         | Default VPC      |
| Availability Zones | a, b, c          |
| Auto Mode          | Disabled         |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Open EKS Console

Go to:

```text id="e2"
AWS Console → EKS → Clusters → Create
```

Choose:

```text id="e3"
Custom Configuration
```

---

# 2️⃣ Configure Cluster Details

| Field              | Value            |
| ------------------ | ---------------- |
| Cluster Name       | `nautilus-eks`   |
| Kubernetes Version | Latest Stable    |
| Cluster IAM Role   | `eksClusterRole` |

---

# 3️⃣ Configure Networking

---

## VPC Selection

Choose:

```text id="e4"
Default VPC
```

---

## Subnet Selection

Select subnets from:

* us-east-1a
* us-east-1b
* us-east-1c

This ensures:

```text id="e5"
High Availability
```

---

# 4️⃣ Configure Cluster Endpoint Access

Under:

```text id="e6"
Cluster Endpoint Access
```

Choose:

| Setting        | Value    |
| -------------- | -------- |
| Public Access  | Disabled |
| Private Access | Enabled  |

---

# 5️⃣ Disable EKS Auto Mode

Under:

```text id="e7"
EKS Auto Mode
```

Set:

```text id="e8"
Disabled
```

---

# 6️⃣ Keep Remaining Settings Default

Leave:

* Logging
* Add-ons
* Encryption
* Node groups

as default unless specified.

---

# 7️⃣ Create Cluster

Click:

```text id="e9"
Create
```

---

# 8️⃣ Wait for Cluster Creation

Go to:

```text id="e10"
EKS → Clusters → nautilus-eks
```

Wait until:

```text id="e11"
Status = Active
```

---

# 9️⃣ Verify Endpoint Access

Under:

```text id="e12"
Networking
```

Verify:

| Endpoint Type | Expected |
| ------------- | -------- |
| Public        | Disabled |
| Private       | Enabled  |

---

# 🔟 Verify Availability Zones

Ensure cluster networking includes:

* us-east-1a
* us-east-1b
* us-east-1c

---

# ✅ Validation Checklist

* ✔ EKS cluster created successfully
* ✔ Latest Kubernetes version selected
* ✔ Private endpoint enabled
* ✔ Public endpoint disabled
* ✔ Default VPC used
* ✔ Multi-AZ subnets selected
* ✔ EKS Auto Mode disabled
* ✔ Cluster status = Active

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Wrong Cluster Creation Mode

### Root Cause

Selected:

```text
Quick Configuration
```

instead of:

```text
Custom Configuration
```

---

### Fix

Use:

```text
Custom Configuration
```

---

## 🔴 Issue 2: Public Endpoint Still Enabled

### Root Cause

Forgot to disable public access.

---

### Fix

Under:

```text
Cluster Endpoint Access
```

Set:

* Public → Disabled
* Private → Enabled

---

## 🔴 Issue 3: IAM Role Missing

### Error

```text
Role not found
```

---

### Fix

Select existing role:

```text
eksClusterRole
```

---

## 🔴 Issue 4: Cluster Creation Failed

### Root Cause

Selected unsupported subnet combination.

---

### Fix

Choose subnets from:

* us-east-1a
* us-east-1b
* us-east-1c

inside default VPC.

---

## 🔴 Issue 5: EKS Auto Mode Enabled Accidentally

### Root Cause

Default option remained enabled.

---

### Fix

Manually disable:

```text
EKS Auto Mode
```

---

## 🔴 Issue 6: Cluster Stuck in Creating State

### Root Cause

IAM role permissions incomplete.

---

### Fix

Ensure:

```text
eksClusterRole
```

contains required EKS policies.

---

## 🔴 Issue 7: Could Not Access Cluster Using kubectl

### Root Cause

Private endpoint allows only VPC/internal access.

---

### Explanation

This is expected behavior for:

```text
Private Endpoint Clusters
```

---

# 🧠 Key Learnings

* EKS provides managed Kubernetes control plane
* Private endpoints improve cluster security
* Multi-AZ networking improves availability
* IAM roles are critical for EKS provisioning
* EKS Auto Mode can be disabled for manual infrastructure control

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* Enterprise Kubernetes platforms
* Secure internal applications
* Microservices deployments
* Regulated workloads
* Production-grade container orchestration

---

# 🏁 Final Outcome

✔ Private EKS cluster deployed successfully
✔ High availability networking configured
✔ Latest Kubernetes version used
✔ Secure private endpoint access configured
✔ Cluster ready for Kubernetes workloads

---

> “A private Kubernetes control plane reduces attack surface and is a foundational best practice for production-grade EKS environments.”
