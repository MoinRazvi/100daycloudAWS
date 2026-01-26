## 📘 Day 01 – Create EC2 Key Pair (AWS)

**File name:** `Day-01-Create-Key-Pair.md`

---

## 🧭 Context

Secure access is the foundation of any cloud infrastructure. Before provisioning compute resources, it is essential to establish a **robust and secure authentication mechanism**. In AWS, EC2 key pairs provide the primary method for secure, passwordless access to Linux-based instances.

Day 01 focused on creating and managing an **EC2 key pair**, ensuring readiness for secure instance access throughout the lifecycle of the infrastructure.

---

## 🎯 Objective

* Establish a secure authentication mechanism for EC2 access
* Create an EC2 key pair using AWS-native best practices
* Prepare for future automation, instance provisioning, and operational access

---

## 🛠️ Implementation Summary

* Created an EC2 key pair via the AWS Management Console
* Selected **RSA** as the encryption algorithm
* Downloaded and securely stored the private key (`.pem`)
* Ensured the key pair is available in the required AWS region

---

## 🔐 Architectural Considerations

* EC2 instances **do not support password-based authentication by default**
* Key pairs eliminate the risks associated with static passwords
* Private keys must be handled as **sensitive credentials**
* Key pairs are **region-specific** and must align with instance placement
* Loss of a private key requires **access recovery procedures** (AMI rebuild, key injection, or instance replacement)

---

## 🧠 Best Practices & Insights

* Treat private keys as secrets (never commit to source control)
* Use distinct key pairs per environment (dev / test / prod)
* Rotate access by replacing instances or re-injecting keys
* Prefer key-based access combined with:

  * Security groups
  * Bastion hosts
  * IAM roles for AWS API access

---

## ✅ Completion Status

* **Lab Status**: Completed
* **AWS Service**: EC2
* **Skill Area**: Security & Access Control
* **Complexity**: Foundational
* **Dependency For**: EC2 provisioning, SSH access, automation workflows

---

## 📌 Key Takeaway

> Strong cloud architectures begin with secure access design.
> EC2 key pairs form the cornerstone of secure compute access in AWS and must be created, managed, and protected with the same rigor as any other credential.

