# 🚀 Day 24 – Setting Up an Application Load Balancer (ALB) for an EC2 Instance

## 🎯 Objective

Set up an **Application Load Balancer (ALB)** in front of an EC2 instance to distribute incoming HTTP traffic, improve availability, and establish a **production-ready entry point** for applications.

This setup reflects a common real-world pattern where direct public access to EC2 instances is avoided in favor of managed load balancing.

---

## 🧠 Why This Matters

Application Load Balancers provide:

* Layer 7 (HTTP/HTTPS) traffic routing
* Health checks and automatic failover
* Integration with Auto Scaling and security controls

Using an ALB decouples client traffic from individual instances, enabling safer scaling and maintenance.

---

## 🛠️ Prerequisites

* A running EC2 instance (example: `xfusion-ec2`) with a web server (Nginx/Apache) listening on port 80
* EC2 instance in a public or private subnet reachable by the ALB
* Permissions to create ALB, target groups, and security groups

---

## 🖥️ Step-by-Step: Create Target Group

### 1️⃣ Open EC2 Service

* AWS Console → Search **EC2** → Click **EC2**

---

### 2️⃣ Navigate to Target Groups

* Left menu → **Load Balancing → Target Groups**
* Click **Create target group**

Configure:

* **Target type**: Instance
* **Target group name**:

  ```
  xfusion-tg
  ```
* **Protocol**: HTTP
* **Port**: 80
* **VPC**: Select EC2 VPC

Health check:

* Protocol: HTTP
* Path: `/`

Click **Next**.

---

### 3️⃣ Register EC2 Instance

* Select `xfusion-ec2`
* Click **Include as pending below**
* Click **Create target group**

---

## 🌐 Step-by-Step: Create Application Load Balancer

### 4️⃣ Navigate to Load Balancers

* EC2 → **Load Balancing → Load Balancers**
* Click **Create load balancer**
* Choose **Application Load Balancer**

---

### 5️⃣ Configure Load Balancer

Basic configuration:

* **Name**:

  ```
  xfusion-alb
  ```
* **Scheme**: Internet-facing
* **IP address type**: IPv4

Network mapping:

* **VPC**: Select VPC
* **Availability Zones**: Select at least two subnets

---

### 🔐 6️⃣ Configure Security Group for ALB

Create or select security group:

* **Name**:

  ```
  xfusion-sg
  ```

Inbound rule:

| Type | Port | Source    |
| ---- | ---- | --------- |
| HTTP | 80   | 0.0.0.0/0 |

Attach this security group to the ALB.

---

### 7️⃣ Configure Listener & Target Group

* Listener: HTTP : 80
* Forward to:

  ```
  xfusion-tg
  ```

Click **Create load balancer**.

---

## 🔄 Step-by-Step: Update EC2 Security Group

### 8️⃣ Allow Traffic from ALB to EC2

Modify EC2 instance security group inbound rules:

| Type | Port | Source                          |
| ---- | ---- | ------------------------------- |
| HTTP | 80   | ALB security group (xfusion-sg) |

> 📌 This ensures EC2 only accepts traffic from the ALB.

---

## ✅ Validation Checklist

* ALB state: **Active**
* Target group health: **Healthy**
* ALB DNS name accessible in browser
* Application page loads successfully

---

## ⚠️ Common Operational Mistakes

* Not registering targets in target group
* Missing EC2 inbound rule from ALB SG
* Using only one subnet for ALB

---

## 🧠 Architectural Insight

In production, EC2 instances should rarely be internet-facing. ALBs provide a **managed, scalable, and secure traffic layer**, enabling blue/green deployments and zero-downtime updates.

---

## 📌 Day 24 Summary

Day 24 focused on deploying an **Application Load Balancer** in front of an EC2 instance, reinforcing modern AWS application architecture patterns.

* Why ALB should front EC2, not direct public exposure
* Proper separation of ALB SG vs EC2 SG
* Target groups, health checks, and listener flow
* Multi-AZ design for high availability
* Common ALB misconfigurations engineers face in real projects
---
