# 🚀 Day 23 – Data Migration Between S3 Buckets Using AWS CLI

## 🎯 Objective

Perform a **complete data migration between two Amazon S3 buckets** using the **AWS CLI**, ensuring **data integrity, consistency, and verification** after transfer.

This exercise reflects a **real-world cloud migration scenario**, where data must be moved safely without loss or corruption.

---

## 🧠 Why This Matters

S3 data migration is a common operational task during:

* Application modernization
* Account or environment separation
* Backup and archival restructuring

Using the AWS CLI provides:

* Speed and automation
* Repeatability
* Verifiable results

---

## 🛠️ Prerequisites

* AWS CLI installed on a host (example: `aws-client`)
* AWS CLI configured with valid credentials
* Source S3 bucket with existing data
* Permission to create and manage S3 buckets

---

## 🖥️ Step-by-Step: Create Destination S3 Bucket

### 1️⃣ Verify AWS CLI Configuration

Run:

```bash
aws sts get-caller-identity
```

This confirms the CLI is authenticated.

---

### 2️⃣ Create New S3 Bucket

```bash
aws s3 mb s3://xfusion-sync-8029
```

> 📌 S3 bucket names must be globally unique.

---

## 🔄 Step-by-Step: Migrate Data Between Buckets

### 3️⃣ Sync Data from Source to Destination

Use the `sync` command to copy all objects:

```bash
aws s3 sync s3://xfusion-s3-23010 s3://xfusion-sync-8029
```

This command:

* Copies all objects
* Preserves directory structure
* Transfers only missing or changed files

---

## 🔍 Step-by-Step: Verify Data Consistency

### 4️⃣ List Objects in Both Buckets

Source bucket:

```bash
aws s3 ls s3://xfusion-s3-23010 --recursive
```

Destination bucket:

```bash
aws s3 ls s3://xfusion-sync-8029 --recursive
```

Compare object count and sizes.

---

### 5️⃣ Optional: Dry-Run Validation

To confirm sync state:

```bash
aws s3 sync s3://xfusion-s3-23010 s3://xfusion-sync-8029 --dryrun
```

No output indicates both buckets are in sync.

---

## ✅ Validation Checklist

* Destination bucket exists
* All objects copied successfully
* Object count and structure match
* No errors during sync

---

## ⚠️ Common Operational Mistakes

* Forgetting region-specific bucket creation requirements
* Using `cp` instead of `sync` for large datasets
* Skipping verification steps

---

## 🧠 Architectural Insight

For large-scale or recurring migrations, consider:

* S3 Replication (CRR / SRR)
* AWS DataSync
* Lifecycle policies instead of manual syncs

CLI-based sync is ideal for **controlled, one-time migrations**.

---

## 📌 Day 23 Summary

Day 23 focused on **migrating data between S3 buckets using AWS CLI**, reinforcing disciplined data handling, verification, and migration best practices.

* EC2 lifecycle & networking
* IAM foundations
* Public infrastructure design
* Secure access
* Storage & data migration
---
