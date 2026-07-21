Excellent progress. You now understand the majority of EBS questions that appear in the SAA exam. Here's a compact summary that you should revise before moving to EFS.

---

# Amazon EBS (Elastic Block Store) - SAA Cheat Sheet

## 1. What is EBS?

* **Persistent block storage** for EC2.
* Similar to a **hard disk (SSD/HDD)** attached to a VM.
* Data **survives EC2 stop/start**.
* Can survive **instance termination** if DeleteOnTermination is disabled.

Think:

> EC2 = Computer
>
> EBS = Hard Disk

---

# 2. EBS vs Instance Store ⭐⭐⭐⭐⭐

| EBS                  | Instance Store                      |
| -------------------- | ----------------------------------- |
| Persistent           | Temporary (Ephemeral)               |
| Network-attached     | Physically attached disk            |
| Can create snapshots | Cannot                              |
| Can encrypt          | Cannot manage encryption separately |
| Survives stop/start  | Lost when instance stops/terminates |
| Used for databases   | Used for cache/temp files           |

Exam keywords:

* **Mission critical** → EBS
* **Temporary cache** → Instance Store
* **Scratch space** → Instance Store

---

# 3. Availability Zone Rule ⭐⭐⭐⭐⭐

An EBS volume **must** be attached to an EC2 instance in the **same Availability Zone**.

Example:

```
EC2 (us-east-1a)
      │
      │
EBS (us-east-1a) ✅

EC2 (us-east-1a)
      │
      │
EBS (us-east-1b) ❌
```

Remember:

> Same Region is NOT enough.
>
> Same Availability Zone is required.

---

# 4. EBS Snapshots ⭐⭐⭐⭐⭐

Snapshot = Backup

```
EBS
   │
Snapshot
   │
Stored internally in S3
```

Important:

* Incremental backup
* Stored in S3 internally
* Cannot manage them like normal S3 objects
* Cannot apply S3 Lifecycle rules to EBS snapshots

Exam trap:

❌ Snapshot → Glacier using lifecycle

Wrong.

---

# 5. Amazon Data Lifecycle Manager (DLM) ⭐⭐⭐⭐⭐

Purpose:

Automatically

* Create snapshots
* Retain snapshots
* Delete old snapshots

Think:

```
DLM
↓

Every night

↓

Take snapshot

↓

Delete after 30 days
```

Exam keywords:

* Automatic
* Simple
* Cheapest
* EBS snapshot automation

Answer:

**Amazon Data Lifecycle Manager**

---

# 6. AWS Backup vs DLM

### DLM

Only EBS snapshots

Simple

Cheap

### AWS Backup

Many AWS services

* EBS
* EFS
* DynamoDB
* RDS
* FSx

Supports

* Cross-account backup
* Cross-region backup
* Compliance
* Central management

Rule:

Only EBS?

→ DLM

Many services/compliance?

→ AWS Backup

---

# 7. EBS Encryption ⭐⭐⭐⭐⭐

Encrypts

* Volumes
* Snapshots
* Data in transit between EC2 and EBS

Uses

AWS KMS

Supported keys

✅ AWS-managed KMS keys

✅ Customer-managed KMS keys

Not supported

❌ Asymmetric keys

---

# 8. Encryption by Default ⭐⭐⭐⭐⭐

Region-level setting

Once enabled

Every new volume becomes encrypted automatically.

Important:

Works only for

New

* Volumes
* Snapshot copies
* Restored volumes

Does NOT encrypt existing volumes.

---

# 9. Live Modification ⭐⭐⭐⭐

You can modify without downtime

* Size
* Volume type
* IOPS

Running production workloads continue.

---

# 10. EBS Replication ⭐⭐⭐⭐

Automatic replication

Within the same AZ only

Not another Region.

People confuse this a lot.

```
AZ-A

EBS

↓

AWS automatically replicates hardware

Inside AZ-A
```

NOT

```
AZ-A

↓

AZ-B

↓

Region-2
```

---

# 11. Volume Types ⭐⭐⭐⭐⭐

## General Purpose SSD (gp3)

Default

Used for

* Web servers
* Boot volumes
* Small databases
* Development

Most common answer.

---

## Provisioned IOPS (io1/io2)

High performance

Used for

* Oracle
* SQL Server
* MySQL
* Large databases

Keywords

* Low latency
* High IOPS
* Database

---

## Magnetic (Previous Generation)

Very cheap

Slow

Used for

* Rarely accessed data

---

# 12. Multi-Attach ⭐⭐⭐

Supported only on

Provisioned IOPS

(io1 / io2 Block Express)

Can attach

One EBS

↓

Multiple EC2

↓

Same AZ only

NOT Multi-AZ.

---

# 13. Common Security Questions

Sensitive data on EBS?

Answer:

✅ Enable EBS Encryption

Sensitive data on S3?

Answer:

✅ Server-side encryption

or

✅ Client-side encryption

---

# 14. Common Backup Questions

Need backups?

Answer:

Snapshot

Need automatic snapshots?

Answer:

DLM

Need enterprise backup?

Answer:

AWS Backup

---

# 15. Exam Traps ⭐⭐⭐⭐⭐

❌ EBS across multiple AZs

Wrong

---

❌ Snapshot lifecycle to Glacier

Wrong

---

❌ EBS automatically replicated to another Region

Wrong

---

❌ EBS can attach to any EC2

Wrong

Same AZ only.

---

❌ Instance Store survives termination

Wrong

Deleted immediately.

---

❌ Encryption uses ACM certificates

Wrong

Uses KMS.

---

# 16. Decision Table

| Requirement             | Service                   |
| ----------------------- | ------------------------- |
| Persistent storage      | EBS                       |
| Temporary storage       | Instance Store            |
| Backup                  | Snapshot                  |
| Automatic snapshots     | DLM                       |
| Enterprise backup       | AWS Backup                |
| Encrypt data            | EBS Encryption (KMS)      |
| Default encryption      | EBS Encryption by Default |
| Database with high IOPS | io1/io2                   |
| Normal workloads        | gp3                       |
| Cheapest legacy storage | Magnetic                  |

---

# Final Memory Tricks

* **EBS = Hard Disk**
* **Instance Store = USB (temporary)**
* **Snapshot = Backup**
* **DLM = Automatic Snapshot Scheduler**
* **AWS Backup = Enterprise Backup Manager**
* **KMS = Encryption**
* **Same AZ = Attach EBS**
* **gp3 = Default**
* **io1/io2 = Database**
* **Magnetic = Cheapest (legacy)**

---

## Readiness Check

You should now be comfortable answering questions about:

* ✅ EBS vs Instance Store
* ✅ Snapshots
* ✅ DLM vs AWS Backup
* ✅ EBS Encryption & KMS
* ✅ Encryption by Default
* ✅ Same-AZ attachment rule
* ✅ Volume types (gp3, io1/io2, Magnetic)
* ✅ Multi-Attach
* ✅ Backup and disaster recovery scenarios

This is a strong foundation. The next topic, **Amazon EFS**, is generally easier because it revolves around one core idea:

> **EBS = One EC2 in one AZ (block storage).**
>
> **EFS = Many EC2 instances across multiple AZs (shared file storage).**

Keeping that comparison in mind will make EFS much easier to learn.



20-July-2026
