# 🚀 Day 28 – Create Amazon ECR Repository and Push Docker Image

## 🎯 Objective

Create a **private Amazon Elastic Container Registry (ECR)** repository, build a Docker image from an existing **Dockerfile**, and push the image to ECR with the **latest** tag.

This lab reflects a **real-world container workflow** used in ECS, EKS, and CI/CD pipelines.

---

## 🧠 Why This Matters

Container images are the foundation of modern application deployment. Amazon ECR provides:

* Secure, private image storage
* Native IAM-based authentication
* Tight integration with ECS, EKS, and AWS CI/CD services

Understanding the full **build → tag → push** flow is essential for any DevOps or Cloud Engineer.

---

## 🛠️ Prerequisites

* Access to the **aws-client** host
* AWS CLI installed and configured
* Docker installed and running
* Dockerfile present at:

  ```
  /root/pyapp/Dockerfile
  ```

---

## 🖥️ Step-by-Step: Create Private ECR Repository

### 1️⃣ Create ECR Repository

Run the following command:

```bash
aws ecr create-repository \
  --repository-name devops-ecr \
  --region us-east-1
```

Note the output value:

```
repositoryUri
```

Example:

```
<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr
```

---

## 🔐 Step-by-Step: Authenticate Docker to ECR

```bash
aws ecr get-login-password --region us-east-1 \
| docker login \
--username AWS \
--password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

Expected output:

```
Login Succeeded
```

---

## 🧩 Step-by-Step: Build Docker Image

### 2️⃣ Navigate to Application Directory

```bash
cd /root/pyapp
```

Verify:

```bash
ls
```

Ensure `Dockerfile` exists.

---

### 3️⃣ Build Docker Image

```bash
docker build -t devops-ecr:latest .
```

> ⚠️ During build, you may see a warning about running pip as root. This is **expected and safe** in container builds.

Verify image:

```bash
docker images
```

---

## 🏷️ Step-by-Step: Tag Image for ECR

```bash
docker tag devops-ecr:latest \
<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

---

## 🚀 Step-by-Step: Push Image to ECR

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

Successful push ends with:

```
latest: digest: sha256:...
```

---

## ✅ Validation Checklist

* ECR repository `devops-ecr` exists
* Docker image built successfully
* Image tagged as `latest`
* Image pushed to ECR
* Image visible in AWS Console → ECR → devops-ecr

---

## ⚠️ Common Operational Mistakes

* Forgetting Docker login to ECR
* Region mismatch between ECR and CLI
* Building image from the wrong directory
* Missing or incorrect image tag

---

## 🧠 Architectural Insight

Containers provide isolation by design, which is why package managers running as root inside Docker images is acceptable. In production environments, further hardening can be applied by using non-root users and multi-stage builds.

---

## 📌 Day 28 Summary

Day 28 focused on implementing a **complete container image lifecycle**—from building a Docker image to securely storing it in Amazon ECR—laying the groundwork for ECS and EKS deployments.

* End-to-end container image lifecycle (build → tag → push)
* Secure image storage using Amazon ECR
* Proper AWS CLI + Docker authentication flow
* Understanding why pip warnings in Docker builds are safe
* Foundations for ECS / EKS deployments
---
