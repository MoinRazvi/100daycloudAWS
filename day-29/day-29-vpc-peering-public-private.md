# 🚀 Day 29 – VPC Peering Between Public and Private VPCs

## 📘 Topic

**Inter-VPC Communication Using VPC Peering (Public ↔ Private Architecture)**

---

## 🎯 Objective

Demonstrate secure private communication between:

* A **public EC2 instance** (`datacenter-public-ec2`) in the **Default VPC**
* A **private EC2 instance** (`datacenter-private-ec2`) in a custom private VPC (`datacenter-private-vpc`)

Using **VPC Peering**, configure routing and security controls so the public EC2 can successfully **ping** the private EC2 over AWS backbone networking.

No NAT, Internet Gateway routing, or VPN should be used for this connectivity.

---

# 🏗️ Existing Infrastructure (Pre‑Created)

## Public Side (Default VPC)

* EC2: `datacenter-public-ec2`
* Subnet: Default public subnet

## Private Side

* VPC: `datacenter-private-vpc`
* CIDR: `10.1.0.0/16`
* Subnet: `datacenter-private-subnet`
* Subnet CIDR: `10.1.1.0/24`
* EC2: `datacenter-private-ec2`

---

# 🖥️ Implementation Steps

---

## 1️⃣ Create VPC Peering Connection

1. Go to **VPC → Peering Connections**
2. Click **Create peering connection**

Configure:

* **Name**:

  ```
  datacenter-vpc-peering
  ```
* **Requester VPC**: Default VPC
* **Accepter VPC**: `datacenter-private-vpc`

Click **Create peering connection**.

---

## 2️⃣ Accept Peering Request

1. Select `datacenter-vpc-peering`
2. Click **Actions → Accept request**

Status must change to:

```
Active
```

---

## 3️⃣ Identify Default VPC CIDR

Go to:

VPC → Your VPCs → Default VPC

Note the CIDR (commonly):

```
172.31.0.0/16
```

You will need this for routing on the private side.

---

## 4️⃣ Update Route Table – Default (Public) VPC

1. Go to **VPC → Route Tables**
2. Identify route table associated with the subnet of `datacenter-public-ec2`
3. Click **Edit routes → Add route**

Add:

```
Destination: 10.1.0.0/16
Target: datacenter-vpc-peering
```

Save.

---

## 5️⃣ Update Route Table – Private VPC

1. Go to **VPC → Route Tables**
2. Select route table associated with `datacenter-private-subnet`
3. Click **Edit routes → Add route**

Add:

```
Destination: <Default-VPC-CIDR>
Target: datacenter-vpc-peering
```

Example:

```
Destination: 172.31.0.0/16
Target: datacenter-vpc-peering
```

Save.

---

## 6️⃣ Update Security Group – Private EC2

Go to EC2 → `datacenter-private-ec2` → Security Group.

Add inbound rule:

| Type | Protocol     | Source           |
| ---- | ------------ | ---------------- |
| ICMP | Echo Request | Default VPC CIDR |

This allows ping from the public EC2.

---

## 7️⃣ Enable SSH Access to Public EC2 (Jump Host)

On **aws-client host**:

```bash
cat /root/.ssh/id_rsa.pub
```

Copy output.

SSH into public EC2 (existing access method), then:

```bash
mkdir -p /home/ec2-user/.ssh
vi /home/ec2-user/.ssh/authorized_keys
```

Paste key and save.

Set permissions:

```bash
chmod 700 /home/ec2-user/.ssh
chmod 600 /home/ec2-user/.ssh/authorized_keys
```

---

## 8️⃣ Test Connectivity

### SSH from aws-client to Public EC2

```bash
ssh ec2-user@<PUBLIC-EC2-IP>
```

---

### Ping Private EC2 from Public EC2

```bash
ping <PRIVATE-EC2-IP>
```

Successful ICMP replies confirm:

* Peering is active
* Routes configured on both sides
* Security groups allow traffic

---

# ✅ Validation Checklist

* Peering status = **Active**
* Route to `10.1.0.0/16` exists in Default VPC route table
* Route to Default VPC CIDR exists in Private VPC route table
* ICMP allowed in private EC2 SG
* Ping works from public EC2 to private EC2

---

# ⚠️ Common Mistakes

* Updating route table on only one side
* Forgetting to accept peering request
* Allowing ICMP from 0.0.0.0/0 instead of specific CIDR
* Expecting transitive routing (not supported)

---

# 🧠 Architectural Insight

VPC Peering:

* Uses AWS backbone networking
* Is non‑transitive
* Requires explicit routing on both VPCs
* Requires non-overlapping CIDRs

For complex hub‑and‑spoke designs, **AWS Transit Gateway** is preferred.

---
