# 🚀 Day 30 – NAT Instance for Private EC2 Internet Access

## 📘 Topic

Private Subnet Internet Access using NAT Instance (AWS + iptables Deep Dive)

---

## 🎯 Objective

Enable outbound internet access for a private EC2 instance using a **NAT Instance**, and validate connectivity by confirming a cron job uploads a file to an S3 bucket.

---

## 🏗️ Architecture Overview

```
Private EC2
   ↓
Private Route Table (0.0.0.0/0 → NAT Instance)
   ↓
NAT Instance (Public Subnet)
   ↓
Internet Gateway
   ↓
Internet / S3
```

---

## 🧱 Existing Setup

* VPC: `devops-priv-vpc`
* Private Subnet: `devops-priv-subnet`
* Private EC2: `devops-priv-ec2`
* S3 Bucket: `devops-nat-10321`
* Cron Job: Uploads file every minute

---

## 🛠️ Implementation Steps

### 1. Create Public Subnet

* Name: `devops-pub-subnet`
* CIDR: `10.1.2.0/24`
* Enable Auto Public IP

---

### 2. Attach Internet Gateway

* Attach IGW to `devops-priv-vpc`

---

### 3. Public Route Table

```
0.0.0.0/0 → Internet Gateway
```

Associate with:

```
devops-pub-subnet
```

---

### 4. Launch NAT Instance

* Name: `devops-nat-instance`
* AMI: Amazon Linux 2023
* Public IP: Enabled
* Subnet: Public subnet

---

### 5. NAT Security Group

#### Inbound:

```
All traffic → 10.1.1.0/24
```

#### Outbound:

```
All traffic → 0.0.0.0/0
```

---

### 6. Disable Source/Destination Check

> Mandatory for NAT functionality

---

### 7. NAT Configuration (Linux)

#### Install iptables

```bash
sudo dnf install iptables-services -y
```

#### Enable IP forwarding

```bash
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward = 1" | sudo tee /etc/sysctl.d/99-nat.conf
sudo sysctl --system
```

#### NAT rule

```bash
sudo iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE
```

#### Forwarding rules

```bash
sudo iptables -F FORWARD
sudo iptables -A FORWARD -i ens5 -o ens5 -j ACCEPT
sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
```

#### Save config

```bash
sudo systemctl enable iptables
sudo systemctl start iptables
sudo service iptables save
```

---

### 8. Private Route Table

```
0.0.0.0/0 → NAT Instance
```

---

## ✅ Validation

Check S3 bucket:

```
devops-nat-10321
```

Expected file:

```
devops-test.txt
```

---

# 🚨 Real Issues Faced & Debugging

## 🔴 Issue 1: NAT Not Working Despite Correct Setup

### Problem

Everything looked correct:

* Routes ✔
* NAT rule ✔
* Security groups ✔

But:

* No internet from private EC2

---

### Root Cause

FORWARD chain had a blocking rule:

```
REJECT all -- reject-with icmp-host-prohibited
```

---

### Why This Broke NAT

Traffic flow:

```
Private EC2 → NAT → REJECT ❌
```

Even though ACCEPT rules existed, they were ignored.

---

### Fix

```
sudo iptables -F FORWARD
sudo iptables -A FORWARD -i ens5 -o ens5 -j ACCEPT
sudo iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo service iptables save
```

---

## 🔴 Issue 2: Rules Disappearing After Restart

### Problem

* ACCEPT rules added
* After restart → gone

---

### Root Cause

iptables service reloads rules from:

```
/etc/sysconfig/iptables
```

Old REJECT rule persisted.

---

### Fix

* Reapply rules
* Save after verification

---

## 🔴 Issue 3: Wrong Interface (eth0 vs ens5)

### Problem

Used:

```
eth0
```

But actual interface:

```
ens5
```

---

### Fix

```
ip a
```

Identify correct interface and update NAT rule.

---

## 🔴 Issue 4: Using `yum` Instead of `dnf`

### Problem

Amazon Linux 2023 uses:

```
dnf
```

Not:

```
yum
```

---

### Fix

```
sudo dnf install iptables-services -y
```

---

## 🔴 Issue 5: Route Table Confusion (Instance vs ENI)

### Observation

Selecting instance auto-converted to ENI.

---

### Insight

This is expected AWS behavior. Not an issue.

---

## 🔴 Issue 6: Misleading Debug Signals

* Ping failing → assumed routing issue
* SSH working → assumed everything fine

---

### Reality

iptables rule order was the actual blocker.

---

# 🧠 Key Learnings

* **iptables rule order is critical**
* NAT depends on:

  * IP forwarding
  * MASQUERADE rule
  * FORWARD chain
* AWS + OS configs must both align
* Debugging requires:

  * Layer-by-layer validation

---

# 🏁 Final Outcome

✔ Private EC2 gained internet access
✔ S3 upload succeeded
✔ NAT instance fully functional

---
