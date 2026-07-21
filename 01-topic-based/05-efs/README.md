Excellent. Before moving to ELB, let's make sure EFS is crystal clear. Fortunately, **EFS is much easier than DynamoDB.**

---

# EFS (Amazon Elastic File System) Summary

## What is EFS?

Amazon EFS is a **managed file storage service**.

Think of it as **a shared network drive**.

Many EC2 instances can mount the same file system simultaneously.

```
EC2-1  \
EC2-2   \
EC2-3 -----> Amazon EFS
EC2-4   /
EC2-5  /
```

All instances see the same files.

---

# Storage Type

EFS = **File Storage**

Remember:

* EBS → Block Storage
* EFS → File Storage
* S3 → Object Storage

---

# Biggest Feature

Thousands of EC2 instances can read/write the same storage at the same time.

This is why EFS exists.

---

# Access

Uses

**NFS (Network File System)**

Especially

**NFSv4**

---

# Supports

✅ Linux

Not Windows.

For Windows use

**Amazon FSx for Windows File Server**

---

# Multi-AZ

Huge exam keyword.

EFS automatically stores data across multiple Availability Zones.

Example

```
AZ-A
 EC2
   \
    \
      EFS

AZ-B
 EC2
   /

AZ-C
 EC2
```

Every instance accesses the same files.

---

# Highly Available

If one AZ fails

Another AZ continues using the same EFS.

---

# Automatically Scales

You never choose storage size.

Starts small

Can grow automatically

GB → TB → PB

---

# POSIX Compliant

Exam keyword.

Means Linux applications work normally.

Supports

* directories
* permissions
* ownership
* file locking

---

# Concurrent Access

One of the biggest clues.

Question says

* many EC2
* shared files
* concurrent access
* multiple AZ

Immediately think

**Amazon EFS**

---

# Performance Modes

There are only **2**.

## 1. General Purpose

Default

Low latency

Most applications

Examples

* websites
* CMS
* WordPress

---

## 2. Max I/O

Higher throughput

Higher latency

Used for

* HPC
* analytics
* hundreds/thousands of clients

Exam keyword:

> Thousands of EC2

→ Max I/O

---

# Throughput Modes

There are **2**.

---

## Bursting Throughput

Default

Performance grows automatically as file size grows.

Good for

* normal workloads
* traffic spikes

---

## Provisioned Throughput

You specify throughput.

Used when

Storage is small

BUT

Traffic is constantly high.

Example

2 TB data

Thousands of requests every second.

Need guaranteed throughput.

---

# ECS Integration

ECS tasks can mount EFS.

```
ECS Task
      \
       \
        Amazon EFS
       /
ECS Task
```

All containers share files.

---

# Typical Use Cases

* WordPress
* CMS
* Shared uploads
* Shared configuration
* ML datasets
* HPC
* Analytics
* Shared container storage
* Thousands of Linux servers

---

# EFS vs EBS vs S3

| Feature              | EBS                   | EFS         | S3      |
| -------------------- | --------------------- | ----------- | ------- |
| Storage Type         | Block                 | File        | Object  |
| Shared               | ❌ (generally one EC2) | ✅ Thousands | Via API |
| Multi-AZ             | ❌ Same AZ             | ✅ Yes       | ✅ Yes   |
| Linux File System    | ❌                     | ✅           | ❌       |
| POSIX                | ❌                     | ✅           | ❌       |
| NFS                  | ❌                     | ✅           | ❌       |
| Automatically Scales | ❌                     | ✅           | ✅       |
| Mountable            | ✅                     | ✅           | ❌       |

---

# Exam Keywords

## If you see:

* shared storage
* file storage
* Linux
* POSIX
* NFS
* multiple EC2
* multiple AZ
* thousands of instances
* concurrent access
* CMS
* WordPress uploads
* ECS shared storage

➡️ **Amazon EFS**

---

## If you see:

* boot volume
* database disk
* one EC2
* block storage
* lowest latency

➡️ **Amazon EBS**

---

## If you see:

* images
* videos
* backups
* archives
* static website
* object storage

➡️ **Amazon S3**

---

# Special Exam Trap

**HPC** does **not always** mean EFS.

Read carefully:

* HPC + **shared file system** + **many EC2** → **EFS**
* HPC + **single EC2** + **very high disk IOPS** → **EBS Provisioned IOPS (io1/io2)**

---

# 30-Second Memory Sheet

* **EFS = Shared Linux file system**
* **File storage**
* **NFSv4**
* **POSIX compliant**
* **Thousands of EC2**
* **Multiple AZs**
* **Automatically scales**
* **General Purpose = low latency (default)**
* **Max I/O = HPC & thousands of clients**
* **Bursting = default throughput**
* **Provisioned = constant high throughput**
* **ECS tasks can mount EFS**
* **Windows → FSx for Windows**
* **Shared storage ⇒ Think EFS**

---

You now have the core EFS concepts needed for the SAA exam. The next topic, **Elastic Load Balancing (ELB)**, is one of the highest-yield services in the exam, so it's worth learning thoroughly.


21-July-2026
