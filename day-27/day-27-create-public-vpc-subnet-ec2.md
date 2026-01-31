# 🚀 Day 27 – Create Public VPC, Subnet, and EC2 Instance

## 🎯 Objective

Create a **public VPC** with a **public subnet**, enable **automatic public IP assignment**, and launch an **EC2 instance** that is reachable over the internet via **SSH (port 22)**.

This day focuses on **network-level fundamentals** required to host public-facing workloads on AWS.

---

## 🧠 Why This Matters

Public-facing applications depend on correct **network design** more than compute itself. A VPC that is incorrectly configured (missing internet gateway, routing, or public IP assignment) results in unreachable workloads even if the EC2 instance is running.

This exercise reinforces:

* VPC isolation
* Public subnet behavior
* Internet reachability fundamentals

---

## 🛠️ Prerequisites

* AWS account with VPC and EC2 permissions
* Basic understanding of CIDR blocks

---

## 🖥️ Step-by-Step: Create Public VPC and Subnet

### 1️⃣ Select Correct Region

* AWS Console → top-right
* Choose required region (example: **us-east-1**)

---

### 2️⃣ Navigate to VPC Dashboard

1. Open **AWS Console**
2. Search for **VPC**
3. Click **VPC**

---

### 3️⃣ Create VPC

* Click **Create VPC**

Configure:

* **Resources to create**: VPC only

* **Name tag**:

  ```
  xfusion-pub-vpc
  ```

* **IPv4 CIDR block**:

  ```
  10.0.0.0/16
  ```

* Click **Create VPC**

---

### 4️⃣ Create Internet Gateway

* Left menu → **Internet Gateways**
* Click **Create internet gateway**

Configure:

* **Name**:

  ```
  xfusion-pub-igw
  ```

* Click **Create internet gateway**

Attach it:

* Select IGW → **Actions → Attach to VPC**
* Choose:

  ```
  xfusion-pub-vpc
  ```

---

### 5️⃣ Create Public Subnet

* Left menu → **Subnets**
* Click **Create subnet**

Configure:

* **VPC**: xfusion-pub-vpc

* **Subnet name**:

  ```
  xfusion-pub-subnet
  ```

* **Availability Zone**: Any

* **CIDR block**:

  ```
  10.0.1.0/24
  ```

* Click **Create subnet**

---

### 6️⃣ Enable Auto-Assign Public IP

* Select **xfusion-pub-subnet**
* Click **Actions → Edit subnet settings**
* Enable:

  ```
  Auto-assign public IPv4 address
  ```
* Click **Save**

---

### 7️⃣ Configure Route Table

* Go to **Route Tables**
* Select the route table associated with xfusion-pub-vpc
* Click **Edit routes**

Add route:

* **Destination**: 0.0.0.0/0
* **Target**: Internet Gateway (xfusion-pub-igw)

Save routes.

Associate subnet:

* **Subnet associations → Edit**
* Select **xfusion-pub-subnet**
* Save

---

## 🖥️ Step-by-Step: Launch Public EC2 Instance

### 8️⃣ Open EC2 Service

* Search **EC2** → Click **EC2**

---

### 9️⃣ Launch Instance

* Click **Launch instances**

Configure:

* **Name**:

  ```
  xfusion-pub-ec2
  ```
* **AMI**: Amazon Linux or Ubuntu
* **Instance type**:

  ```
  t2.micro
  ```

---

### 🔐 1️⃣0️⃣ Network & Security

Under **Network settings**:

* **VPC**: xfusion-pub-vpc
* **Subnet**: xfusion-pub-subnet

Create security group:

* **Name**: xfusion-pub-sg

Inbound rules:

| Type | Port | Source    |
| ---- | ---- | --------- |
| SSH  | 22   | 0.0.0.0/0 |

> 🔐 Restrict SSH in production; open access is acceptable for this lab.

---

### 1️⃣1️⃣ Launch Instance

* Select a key pair
* Click **Launch instance**

---

## ✅ Validation Checklist

* VPC exists: xfusion-pub-vpc
* Subnet exists: xfusion-pub-subnet
* Auto-assign public IP enabled
* Internet Gateway attached
* Route table has 0.0.0.0/0 → IGW
* EC2 instance state: Running
* EC2 has a public IPv4 address
* SSH (port 22) reachable from the internet

---

## ⚠️ Common Operational Mistakes

* Forgetting to attach Internet Gateway
* Missing route to IGW
* Not enabling auto-assign public IP
* Launching EC2 in wrong subnet

---

## 🧠 Architectural Insight

A **public subnet** is defined by routing, not by name. Public IP assignment + IGW routing together enable true internet reachability.

---

## 🖼️ Public VPC Architecture – Visual Explanation

Internet
|
|
+--▼---------------------------+
| Internet Gateway |
| (xfusion-pub-igw) |
+--▲---------------------------+
|
| 0.0.0.0/0
+--▼---------------------------+
| Route Table |
| (Public Route Table) |
| 0.0.0.0/0 → IGW |
+--▲---------------------------+
|
| Associated
+--▼---------------------------+
| Public Subnet |
| xfusion-pub-subnet |
| Auto-assign Public IP = ON |
+--▲---------------------------+
|
|
+--▼---------------------------+
| EC2 Instance |
| xfusion-pub-ec2 |
| Public IPv4 Assigned |
| SG: SSH (22) open |
+------------------------------+


---

## 🔍 How Internet Access Works (Step-by-Step)

### 1️⃣ Internet Gateway (IGW)
Acts as the bridge between your **VPC** and the **public internet**.

---

### 2️⃣ Route Table
The route:



0.0.0.0/0 → Internet Gateway tells AWS:

> “Send all non-local traffic to the internet.”

---

### 3️⃣ Public Subnet
A subnet becomes **public** only because:

- It is associated with a route table pointing to the **Internet Gateway**
- **Auto-assign public IPv4** is enabled

---

### 4️⃣ EC2 Instance
Receives:

- A **public IPv4 address**
- Internet reachability via **IGW**
- SSH access controlled by the **Security Group**

---

## 🧠 Key Design Rule (Very Important)

> A subnet is **not public by name**  
> A subnet is public **only by routing**

Most AWS networking issues happen when **one of these is missing**:

- Internet Gateway attached to VPC  
- Route table entry pointing to IGW  
- Subnet associated with the route table  
- Public IP assignment enabled  

---

## 📌 Day 27 Summary

Day 27 focused on building a **public network foundation** by creating a VPC, public subnet, and internet-accessible EC2 instance, reinforcing core AWS networking principles.

* What actually makes a subnet public (IGW + route + public IP)
* VPC and subnet design from first principles
* Internet Gateway and route table association
* Launching an internet-accessible EC2 safely
* Avoiding classic networking misconfigurations
  
---
