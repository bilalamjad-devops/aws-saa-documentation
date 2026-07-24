You're welcome! Before moving to S3, let's summarize **RDS** into a one-page cheat sheet. This is enough to answer **80–90% of SAA RDS questions**.

---

# Amazon RDS Summary

## 1. What is RDS?

Amazon RDS is a **managed relational database service**.

AWS handles:

* OS patching
* Backups
* Monitoring
* Failover
* Maintenance

You manage:

* Database
* Tables
* Queries
* Users

---

# 2. Multi-AZ vs Read Replica ⭐⭐⭐⭐⭐

This is the #1 RDS exam topic.

| Multi-AZ                | Read Replica             |
| ----------------------- | ------------------------ |
| High Availability       | Performance              |
| Synchronous replication | Asynchronous replication |
| Automatic failover      | No automatic failover    |
| Disaster recovery       | Read scaling             |
| Standby database        | Read-only database       |

---

Remember:

```text
Multi-AZ = Availability
```

```text
Read Replica = Performance
```

---

# 3. Multi-AZ

Architecture

```text
Application

      |

Primary DB (AZ-1)

      ||

 Synchronous

      ||

Standby DB (AZ-2)
```

Purpose

* High Availability
* Automatic Failover

Standby cannot serve reads.

---

### Multi-AZ Failover occurs if:

* Primary storage fails
* Primary compute fails
* Primary Availability Zone fails
* Primary network fails

---

### No failover if:

* Standby fails
* Read Replica fails

---

### During failover

AWS changes

```text
DNS (CNAME)

NOT

IP Address
```

---

# 4. Read Replica

Purpose

Read-heavy workloads

Example

Millions of users reading articles.

Architecture

```text
Primary

     |

Read Replica

Read Replica

Read Replica
```

Benefits

* Improves read performance
* Asynchronous replication
* Read-only

---

Not used for

* Automatic failover
* High availability

---

# 5. Database Migration

Moving database to AWS?

Use

✅ AWS DMS

---

Changing database engine?

Example

Oracle

↓

PostgreSQL

Use

✅ AWS SCT

*

✅ AWS DMS

---

Easy memory

```text
DMS

Moves Data
```

```text
SCT

Changes Schema
```

---

# 6. IAM DB Authentication

Instead of

```text
Username

Password
```

Use

```text
IAM Token
```

Benefits

* Temporary authentication token
* No stored password
* Uses EC2 IAM Role

Supported

* MySQL
* PostgreSQL

---

# 7. SSL

Question says

> Secure data in transit

Answer

SSL

Examples

* rds.force_ssl = true
* Install RDS Root CA certificate

---

Remember

```text
In Transit

↓

SSL
```

---

# 8. Encryption at Rest

Protect stored data.

Use

* AWS KMS
* TDE (SQL Server, Oracle)

---

Remember

```text
At Rest

↓

KMS / TDE
```

---

# 9. Common Exam Keywords

| Keyword                     | Answer                |
| --------------------------- | --------------------- |
| High Availability           | Multi-AZ              |
| Automatic Failover          | Multi-AZ              |
| Synchronous                 | Multi-AZ              |
| Read-heavy                  | Read Replica          |
| Read Scaling                | Read Replica          |
| Authentication Token        | IAM DB Authentication |
| Secure data in transit      | SSL                   |
| Encrypt stored data         | KMS / TDE             |
| Move database               | DMS                   |
| Convert Oracle → PostgreSQL | SCT + DMS             |

---

# 10. 5-Second Decision Tree

Need High Availability?

↓

✅ Multi-AZ

---

Need Faster Reads?

↓

✅ Read Replica

---

Need Database Migration?

↓

✅ DMS

---

Need Different Database Engine?

↓

✅ SCT + DMS

---

Need Token instead of Password?

↓

✅ IAM DB Authentication

---

Need Encrypt Network Traffic?

↓

✅ SSL

---

Need Encrypt Stored Data?

↓

✅ KMS / TDE

---

# Final RDS Mind Map

```text
                    Amazon RDS
                         │
        ┌────────────────┼────────────────┐
        │                │                │
   Multi-AZ        Read Replica      Migration
        │                │                │
 High Availability   Read Scaling      AWS DMS
 Synchronous         Asynchronous      SCT (if engine changes)
 Automatic Failover  Read-only
        │
   Failover = CNAME changes
        │
 ┌──────┴─────────────┐
 │                    │
SSL              IAM DB Auth
 │                    │
Data in Transit   IAM Token (No Password)
```

---

## Ready for S3 🚀

S3 is one of the **highest-scoring topics** in the SAA exam. The questions repeat a lot, and once you understand the patterns, they're much easier than RDS.

The major topics we'll cover are:

1. **Storage Classes** (Standard, IA, One Zone-IA, Glacier, Deep Archive)
2. **Versioning**
3. **Lifecycle Policies**
4. **Encryption (SSE-S3, SSE-KMS, SSE-C)**
5. **Bucket Policies vs IAM Policies vs ACLs**
6. **Pre-signed URLs**
7. **Cross-Region Replication (CRR)**
8. **Transfer Acceleration**
9. **Event Notifications**
10. **Object Lock & MFA Delete**
11. **Storage Gateway & Snow Family (where relevant)**

Once you master these, you'll be able to solve the vast majority of S3 questions confidently.



23-July-2026
