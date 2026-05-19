# 🚀 Day 41 – AWS KMS Encryption & Decryption of Sensitive Data

## 📘 Topic

Encrypting and decrypting sensitive files using AWS Key Management Service (KMS).

---

# 🎯 Objective

The Nautilus DevOps team needs to improve data security by:

* Creating a symmetric AWS KMS key
* Encrypting a sensitive file
* Base64 encoding the encrypted data
* Decrypting the file for verification
* Validating encryption integrity

This task demonstrates:

* AWS KMS usage
* Symmetric encryption
* AWS CLI encryption workflows
* Base64 ciphertext handling
* Secure file encryption practices

---

# 🏗️ Architecture Overview

```text id="k1"
SensitiveData.txt
        ↓
AWS KMS Key
(datacenter-KMS-Key)
        ↓
Encryption
        ↓
EncryptedData.bin
(Base64 Ciphertext)
        ↓
Decryption
        ↓
Original Data Verified
```

---

# 🧱 Requirements

| Component      | Value                     |
| -------------- | ------------------------- |
| KMS Key        | `datacenter-KMS-Key`      |
| Source File    | `/root/SensitiveData.txt` |
| Encrypted File | `/root/EncryptedData.bin` |
| Key Type       | Symmetric                 |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create Symmetric KMS Key

Go to:

```text id="k2"
AWS Console → KMS → Customer Managed Keys
```

Click:

```text id="k3"
Create Key
```

---

## Configuration

| Field     | Value               |
| --------- | ------------------- |
| Key Type  | Symmetric           |
| Key Usage | Encrypt and Decrypt |

---

## Alias

Set:

```text id="k4"
datacenter-KMS-Key
```

Complete key creation.

---

# 2️⃣ Verify Source File

On aws-client host:

```bash id="k5"
ls /root/SensitiveData.txt
```

View contents:

```bash id="k6"
cat /root/SensitiveData.txt
```

---

# 3️⃣ Encrypt File Using AWS CLI

Run:

```bash id="k7"
aws kms encrypt \
--key-id alias/datacenter-KMS-Key \
--plaintext fileb:///root/SensitiveData.txt \
--query CiphertextBlob \
--output text > /root/EncryptedData.bin
```

---

# ✅ What This Does

| Option           | Purpose                      |
| ---------------- | ---------------------------- |
| `fileb://`       | Reads binary file            |
| `CiphertextBlob` | Returns encrypted ciphertext |
| `--output text`  | Base64 encoded output        |
| `>`              | Saves encrypted data         |

---

# 4️⃣ Verify Encrypted File

Check file exists:

```bash id="k8"
ls /root/EncryptedData.bin
```

View encrypted data:

```bash id="k9"
cat /root/EncryptedData.bin
```

Expected:

```text id="k10"
Base64 encoded ciphertext
```

---

# 5️⃣ Decrypt Encrypted File

Run:

```bash id="k11"
aws kms decrypt \
--ciphertext-blob fileb://<(base64 -d /root/EncryptedData.bin) \
--query Plaintext \
--output text | base64 -d
```

---

# ✅ Expected Output

Should display original contents of:

```text id="k12"
SensitiveData.txt
```

---

# 6️⃣ Verify Decrypted Data Matches Original

Compare manually or run:

```bash id="k13"
diff /root/SensitiveData.txt <(
aws kms decrypt \
--ciphertext-blob fileb://<(base64 -d /root/EncryptedData.bin) \
--query Plaintext \
--output text | base64 -d
)
```

No output means:

```text id="k14"
Files match successfully
```

---

# ✅ Validation Checklist

* ✔ Symmetric KMS key created
* ✔ Alias configured correctly
* ✔ Sensitive file encrypted successfully
* ✔ Ciphertext stored in EncryptedData.bin
* ✔ Base64 encoding completed
* ✔ Decryption validated successfully

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Incorrect Key Type

### Root Cause

Created asymmetric key accidentally.

---

### Fix

Create:

```text id="k15"
Symmetric
```

key only.

---

## 🔴 Issue 2: Validation Script Failed

### Root Cause

Encrypted file not base64 encoded properly.

---

### Fix

Use:

```bash id="k16"
--output text
```

during encryption.

---

## 🔴 Issue 3: File Not Found Error

### Error

```text id="k17"
No such file or directory
```

---

### Fix

Verify source file:

```bash id="k18"
ls /root/SensitiveData.txt
```

---

## 🔴 Issue 4: AccessDeniedException

### Root Cause

IAM permissions missing for KMS operations.

---

### Fix

Ensure permissions include:

* kms:Encrypt
* kms:Decrypt

---

## 🔴 Issue 5: Decryption Output Garbled

### Root Cause

Forgot base64 decoding.

---

### Fix

Use:

```bash id="k19"
base64 -d
```

during decryption.

---

## 🔴 Issue 6: Wrong Key Alias Used

### Root Cause

CLI command referenced incorrect alias.

---

### Correct Alias

```text id="k20"
alias/datacenter-KMS-Key
```

---

## 🔴 Issue 7: CiphertextBlob Missing

### Root Cause

Incorrect query option.

---

### Correct Command

```bash id="k21"
--query CiphertextBlob
```

---

# 🧠 Key Learnings

* KMS securely manages encryption keys
* Symmetric keys are used for encryption/decryption
* CiphertextBlob contains encrypted data
* Base64 encoding enables safe text storage
* AWS CLI integrates directly with KMS

---

# 🔍 Real-World Use Cases

This workflow is commonly used for:

* Secure secrets storage
* Database backup encryption
* Compliance requirements
* File protection
* Application secrets management

---

# 🏁 Final Outcome

✔ KMS symmetric key created successfully
✔ Sensitive file encrypted securely
✔ EncryptedData.bin generated correctly
✔ Decryption validated successfully

---
