# 🚀 Day 42 – Building a Simple To-Do Application Backend Using DynamoDB

## 📘 Topic

Creating and managing a DynamoDB table for a To-Do application using AWS NoSQL database services.

---

# 🎯 Objective

The Nautilus DevOps team needs to:

* Create a DynamoDB table
* Define a primary key
* Insert task records
* Store task status information
* Validate inserted data

This task demonstrates:

* DynamoDB table creation
* NoSQL database concepts
* Primary key design
* Item insertion
* Data verification using AWS Console/CLI

---

# 🏗️ Architecture Overview

```text id="dy1"
To-Do Application
        ↓
Amazon DynamoDB
(nautilus-tasks)
        ↓
Task Items
(taskId, description, status)
```

---

# 🧱 Requirements

| Component     | Value            |
| ------------- | ---------------- |
| Table Name    | `nautilus-tasks` |
| Partition Key | `taskId`         |
| Key Type      | String           |
| Task 1 Status | `completed`      |
| Task 2 Status | `in-progress`    |

---

# 🛠️ Implementation Steps

---

# 1️⃣ Create DynamoDB Table

Go to:

```text id="dy2"
AWS Console → DynamoDB → Tables → Create Table
```

---

## Table Configuration

| Field         | Value            |
| ------------- | ---------------- |
| Table Name    | `nautilus-tasks` |
| Partition Key | `taskId`         |
| Data Type     | String           |

---

## Capacity Settings

Keep:

```text id="dy3"
Default settings
```

Click:

```text id="dy4"
Create Table
```

---

# 2️⃣ Wait for Table Creation

Ensure status becomes:

```text id="dy5"
Active
```

---

# 3️⃣ Insert Task 1

Go to:

```text id="dy6"
nautilus-tasks → Explore table items → Create item
```

Add:

```json id="dy7"
{
  "taskId": "1",
  "description": "Learn DynamoDB",
  "status": "completed"
}
```

Save item.

---

# 4️⃣ Insert Task 2

Create another item:

```json id="dy8"
{
  "taskId": "2",
  "description": "Build To-Do App",
  "status": "in-progress"
}
```

Save item.

---

# 5️⃣ Verify Inserted Data

Go to:

```text id="dy9"
Explore table items
```

Verify:

| taskId | description     | status      |
| ------ | --------------- | ----------- |
| 1      | Learn DynamoDB  | completed   |
| 2      | Build To-Do App | in-progress |

---

# ✅ Validation Checklist

* ✔ DynamoDB table created successfully
* ✔ Primary key configured correctly
* ✔ Task 1 inserted
* ✔ Task 2 inserted
* ✔ Status values verified correctly
* ✔ Table state = Active

---

# 🚨 Real Issues Faced & Troubleshooting

---

## 🔴 Issue 1: Wrong Primary Key Type

### Root Cause

Selected Number instead of String.

---

### Fix

Use:

| Key    | Type   |
| ------ | ------ |
| taskId | String |

---

## 🔴 Issue 2: Table Creation Failed

### Root Cause

Duplicate table name.

---

### Fix

Ensure table name is exactly:

```text id="dy10"
nautilus-tasks
```

---

## 🔴 Issue 3: Invalid JSON Format

### Error

```text id="dy11"
JSON syntax error
```

---

### Fix

Use proper JSON:

```json id="dy12"
{
  "taskId": "1",
  "description": "Learn DynamoDB",
  "status": "completed"
}
```

---

## 🔴 Issue 4: Missing taskId

### Root Cause

Primary key attribute missing.

---

### Fix

Every item MUST include:

```text id="dy13"
taskId
```

---

## 🔴 Issue 5: Wrong Status Value

### Problem

Status typed incorrectly.

---

### Correct Values

Task 1:

```text id="dy14"
completed
```

Task 2:

```text id="dy15"
in-progress
```

---

## 🔴 Issue 6: Table Still Creating

### Root Cause

Attempted insertion before table became active.

---

### Fix

Wait until:

```text id="dy16"
Status = Active
```

---

# 🧠 Key Learnings

* DynamoDB is a NoSQL key-value database
* Tables require primary keys
* DynamoDB scales automatically
* JSON structure is used for item storage
* DynamoDB is schema-flexible beyond primary keys

---

# 🔍 Real-World Use Cases

This architecture is commonly used for:

* To-Do applications
* User sessions
* Shopping carts
* Serverless applications
* Gaming leaderboards

---

# 🏁 Final Outcome

✔ DynamoDB table created successfully
✔ Task records inserted correctly
✔ Task statuses validated successfully
✔ To-Do application backend configured

---

# 💡 DevOps Insight

> “DynamoDB enables highly scalable applications without managing database servers, making it ideal for modern cloud-native workloads.”
