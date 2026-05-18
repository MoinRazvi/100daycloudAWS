# 🚀 Day 38 – Deploying Containerized Application Using AWS ECR + ECS Fargate

## 📘 Topic

Building and deploying a Dockerized application using Amazon ECR and Amazon ECS Fargate.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create a private Amazon ECR repository
* Build Docker image from Dockerfile
* Push image to ECR
* Create ECS Fargate cluster
* Create ECS task definition
* Deploy ECS service running containerized application

This task demonstrates:

* Container image management
* Docker + ECR integration
* ECS Fargate deployment
* Serverless container orchestration
* Production-style container deployment workflow

---

# 🏗️ Architecture Overview

```text id="m1"
Developer (aws-client)
        ↓
Docker Build
        ↓
Amazon ECR (Private Repository)
        ↓
ECS Task Definition
        ↓
ECS Cluster (Fargate)
        ↓
ECS Service
        ↓
Running Container Task
```

---

# 🧱 Requirements

| Component       | Value                       |
| --------------- | --------------------------- |
| ECR Repository  | `datacenter-ecr`            |
| ECS Cluster     | `datacenter-cluster`        |
| Task Definition | `datacenter-taskdefinition` |
| ECS Service     | `datacenter-service`        |
| Launch Type     | Fargate                     |
| Image Tag       | `latest`                    |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Private ECR Repository

Go to:

```text id="m2"
AWS Console → ECR → Create Repository
```

---

## Configuration

| Field           | Value            |
| --------------- | ---------------- |
| Visibility      | Private          |
| Repository Name | `datacenter-ecr` |

Create repository.

---

# 2️⃣ Navigate to Dockerfile Location

On `aws-client` host:

```bash id="m3"
cd /root/pyapp
```

Verify Dockerfile:

```bash id="m4"
ls
```

Expected:

```text id="m5"
Dockerfile
```

---

# 3️⃣ Build Docker Image

```bash id="m6"
docker build -t datacenter-ecr:latest .
```

Verify image:

```bash id="m7"
docker images
```

---

# 4️⃣ Authenticate Docker to ECR

Retrieve login command:

```bash id="m8"
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com
```

Expected:

```text id="m9"
Login Succeeded
```

---

# 5️⃣ Tag Docker Image

```bash id="m10"
docker tag datacenter-ecr:latest <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr:latest
```

---

# 6️⃣ Push Docker Image to ECR

```bash id="m11"
docker push <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr:latest
```

Expected:

```text id="m12"
latest: digest: sha256:xxxx
```

---

# 7️⃣ Create ECS Cluster

Go to:

```text id="m13"
AWS Console → ECS → Clusters → Create Cluster
```

---

## Configuration

| Field          | Value                |
| -------------- | -------------------- |
| Cluster Name   | `datacenter-cluster` |
| Infrastructure | AWS Fargate          |

Create cluster.

---

# 8️⃣ Create ECS Task Definition

Go to:

```text id="m14"
ECS → Task Definitions → Create
```

---

## Configuration

| Field                | Value                       |
| -------------------- | --------------------------- |
| Launch Type          | Fargate                     |
| Task Definition Name | `datacenter-taskdefinition` |
| CPU                  | 0.25 vCPU                   |
| Memory               | 0.5 GB                      |

---

## Add Container

| Field          | Value                  |
| -------------- | ---------------------- |
| Container Name | `datacenter-container` |
| Image URI      | ECR Image URI          |
| Port Mapping   | 80                     |

---

# 9️⃣ Create ECS Service

Go to:

```text id="m15"
ECS → Clusters → datacenter-cluster → Create Service
```

---

## Service Configuration

| Field           | Value                       |
| --------------- | --------------------------- |
| Launch Type     | Fargate                     |
| Task Definition | `datacenter-taskdefinition` |
| Service Name    | `datacenter-service`        |
| Desired Tasks   | 1                           |

---

## Networking

Choose:

* Default VPC
* Public Subnets
* Auto assign public IP → Enabled

---

# 🔟 Verify Running Task

Go to:

```text id="m16"
ECS → Clusters → datacenter-cluster → Tasks
```

Ensure:

```text id="m17"
Status = Running
```

---

# ✅ Validation Checklist

* ✔ Private ECR repository created
* ✔ Docker image built successfully
* ✔ Image pushed to ECR
* ✔ ECS cluster created
* ✔ ECS task definition configured
* ✔ ECS service deployed
* ✔ ECS task running successfully

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Docker Login Failed

### Error

```text id="m18"
no basic auth credentials
```

---

### Root Cause

Docker not authenticated with ECR.

---

### Fix

Run:

```bash id="m19"
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

## 🔴 Issue 2: Push Access Denied

### Root Cause

Incorrect image tag.

---

### Fix

Correct tagging format:

```bash id="m20"
docker tag datacenter-ecr:latest <ACCOUNT-ID>.dkr.ecr.us-east-1.amazonaws.com/datacenter-ecr:latest
```

---

## 🔴 Issue 3: ECS Task Stopped Immediately

### Root Cause

Container port mismatch or application crash.

---

### Fix

Verify:

* Container starts properly
* Correct port mapping configured
* Application listening on expected port

---

## 🔴 Issue 4: ECS Service Fails to Launch Tasks

### Root Cause

Fargate networking configuration missing.

---

### Fix

Enable:

```text id="m21"
Auto assign public IP
```

and choose:

```text id="m22"
Public subnets
```

---

## 🔴 Issue 5: Image Not Found in ECS

### Root Cause

Incorrect ECR URI in task definition.

---

### Fix

Use exact URI from:

```text id="m23"
ECR → View Push Commands
```

---

## 🔴 Issue 6: Docker Build Warnings

### Warning

```text id="m24"
Running pip as root user
```

---

### Explanation

Common warning during Docker image builds.

Usually safe for lab environments.

---

## 🔴 Issue 7: ECS Task Pending Forever

### Root Cause

Insufficient networking/security configuration.

---

### Fix

Verify:

* VPC selected correctly
* Public subnet selected
* Security groups allow outbound traffic

---

# 🧠 Key Learnings

* ECR securely stores container images
* ECS Fargate removes server management overhead
* Task Definitions define container runtime behavior
* ECS Services maintain desired task count
* Container orchestration simplifies deployments

---

# 🔍 Real-World Use Cases

This architecture is widely used for:

* Microservices deployments
* API hosting
* Containerized web applications
* Scalable backend services
* Serverless container workloads

---

# 🏁 Final Outcome

✔ Private ECR repository created successfully
✔ Docker image built and pushed successfully
✔ ECS Fargate cluster configured
✔ ECS service deployed successfully
✔ Running container task validated

---
