# 🚀 Day 50 – Expand EC2 Root Volume from 8 GiB to 12 GiB

## 📘 Topic

Expanding AWS EBS root volume and resizing Linux filesystem without downtime.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Identify the EBS volume attached to:

  ```text
  nautilus-ec2
  ```
* Increase storage from:

  ```text
  8 GiB → 12 GiB
  ```
* Expand the root filesystem inside the EC2 instance
* Ensure:

  ```text
  /
  ```

  partition reflects new storage size

This task demonstrates:

* EBS volume expansion
* Linux partition resizing
* Filesystem expansion
* Live storage scaling on EC2

---

# 🏗️ AWS Architecture Overview

```text id="d50a1"
                AWS EC2 Instance
                  nautilus-ec2
                         │
                         ▼
               Root EBS Volume
                     8 GiB
                         │
               Modify Volume Size
                         ▼
                    12 GiB
                         │
             Linux Filesystem Resize
                         ▼
              Root Partition Expanded
                         ▼
                     / = 12 GiB
```

---

# 🛠️ Step-by-Step Implementation

---

# 1️⃣ Identify EC2 Attached Volume

Go to:

```text id="d50a2"
EC2 Console → Instances → nautilus-ec2
```

Under:

```text id="d50a3"
Storage
```

Note:

* Volume ID
* Current size:

  ```text
  8 GiB
  ```

---

# 2️⃣ Modify EBS Volume

Click volume ID.

Then:

```text id="d50a4"
Actions → Modify Volume
```

Change:

| Current | New    |
| ------- | ------ |
| 8 GiB   | 12 GiB |

Click:

```text id="d50a5"
Modify
```

Confirm modification.

---

# 3️⃣ Wait for Volume Modification

Go to:

```text id="d50a6"
Elastic Block Store → Volumes
```

Wait until:

```text id="d50a7"
Optimizing
```

or

```text id="d50a8"
Completed
```

---

# 4️⃣ SSH into EC2 Instance

From aws-client host:

```bash id="d50c1"
ssh -i /root/nautilus-keypair.pem ubuntu@<PUBLIC-IP>
```

Replace:

```text id="d50a9"
<PUBLIC-IP>
```

with EC2 public IP.

---

# 5️⃣ Verify Current Disk Size

Run:

```bash id="d50c2"
lsblk
```

You should now see:

```text id="d50a10"
12G
```

for root EBS device.

---

# 6️⃣ Expand Root Partition

For Ubuntu instances run:

```bash id="d50c3"
sudo growpart /dev/xvda 1
```

If device is:

```text id="d50a11"
/dev/nvme0n1
```

then run:

```bash id="d50c4"
sudo growpart /dev/nvme0n1 1
```

---

# 7️⃣ Resize Filesystem

For Ubuntu ext4 filesystem:

```bash id="d50c5"
sudo resize2fs /dev/xvda1
```

OR if NVMe:

```bash id="d50c6"
sudo resize2fs /dev/nvme0n1p1
```

---

# 8️⃣ Verify Expanded Root Partition

Run:

```bash id="d50c7"
df -h
```

Expected:

```text id="d50a12"
/ ≈ 12G
```

---

# ✅ Final Validation Checklist

* ✔ EBS volume identified
* ✔ Volume expanded from 8 GiB to 12 GiB
* ✔ Partition resized successfully
* ✔ Filesystem expanded successfully
* ✔ Root partition reflects new size
* ✔ No downtime occurred

---

# 🚨 Real Troubleshooting Issues Faced

---

# 🔴 Issue 1: Volume Size Changed But OS Still Shows 8G

## Root Cause

Filesystem not resized.

---

## Fix

Run:

```bash id="d50c8"
sudo resize2fs /dev/xvda1
```

---

# 🔴 Issue 2: growpart Command Not Found

## Root Cause

cloud-guest-utils package missing.

---

## Fix

Install package:

```bash id="d50c9"
sudo apt update
sudo apt install cloud-guest-utils -y
```

---

# 🔴 Issue 3: Wrong Device Name Used

## Root Cause

Different AWS instance types use:

* xvda
  OR
* nvme0n1

---

## Fix

Verify device using:

```bash id="d50c10"
lsblk
```

---

# 🔴 Issue 4: resize2fs Failed

## Root Cause

Wrong partition selected.

---

## Fix

Use:

```text id="d50a13"
Partition device
```

NOT entire disk.

Correct examples:

* `/dev/xvda1`
* `/dev/nvme0n1p1`

---

# 🔴 Issue 5: Volume Modification Still In Progress

## Root Cause

AWS optimization delay.

---

## Fix

Wait until:

```text id="d50a14"
Optimizing
```

or

```text id="d50a15"
Completed
```

before resizing filesystem.

---

# 🔴 Issue 6: SSH Permission Denied

## Root Cause

Incorrect key permissions.

---

## Fix

Run:

```bash id="d50c11"
chmod 400 /root/nautilus-keypair.pem
```

---

# 🧠 Key Learnings

* EBS volumes can be expanded without downtime
* Linux partitions must be resized manually
* Filesystems do not auto-expand
* growpart expands partition tables
* resize2fs expands ext4 filesystems

---

# 🔍 Real-World Use Cases

This workflow is commonly used for:

* Production disk expansion
* Database storage growth
* Application scaling
* Log storage expansion
* Live infrastructure scaling

---

# 🏁 Final Outcome

✔ Root EBS volume expanded successfully
✔ Linux filesystem resized correctly
✔ Root partition now reflects 12 GiB
✔ Storage expansion completed without interruption

```

# 💡 DevOps Insight

> “Cloud-native infrastructure allows storage scaling without downtime, but operating systems still require filesystem-level expansion to utilize the new capacity.”
