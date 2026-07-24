Great! Here's a concise **S3 summary** focused on the concepts that repeatedly appear in the SAA exam.

# Amazon S3 Summary

## 1. What is S3?

* Object storage
* Unlimited storage
* 11 9's (99.999999999%) durability
* Stores files (objects) inside buckets

---

# Storage Classes

| Storage Class                     | Use Case                                                       |
| --------------------------------- | -------------------------------------------------------------- |
| **S3 Standard**                   | Frequently accessed data                                       |
| **S3 Standard-IA**                | Infrequent access but instant retrieval                        |
| **S3 One Zone-IA**                | Infrequent access, instant retrieval, easily reproducible data |
| **S3 Intelligent-Tiering**        | Unknown/changing access patterns                               |
| **S3 Glacier Instant Retrieval**  | Rarely accessed, milliseconds retrieval                        |
| **S3 Glacier Flexible Retrieval** | Archive, minutes to hours retrieval                            |
| **S3 Glacier Deep Archive**       | Cheapest, long-term archive (hours retrieval)                  |

### Memory Trick

* Standard → Daily use
* Standard-IA → Sometimes
* One Zone-IA → Cheap + reproducible
* Intelligent → AWS decides
* Glacier → Archive

---

# Lifecycle Policies

Automatically move objects between storage classes.

Example:

```
30 Days
↓

Standard → Standard-IA

365 Days
↓

Glacier
```

No EC2 or cron jobs needed.

---

# Versioning

Purpose:

* Recover deleted files
* Recover overwritten files

Think:

> Git for S3

---

# MFA Delete

Requires MFA before:

* Permanently deleting object versions
* Disabling Versioning

Usually paired with Versioning.

---

# Encryption

### SSE-S3

* AWS manages keys
* AES-256

### SSE-KMS

* AWS KMS manages keys
* More control
* Auditing

### SSE-C

* Customer provides encryption key to AWS for each request

### Client-Side Encryption

Encrypt **before** sending to AWS.

If the question says:

> Data and master key must never reach AWS

Answer:

✅ Client-side encryption with a client-side master key.

---

# S3 + CloudFront

Need:

* Durable storage
* Global distribution
* Fast downloads

Answer:

```
S3
↓

CloudFront
```

---

# Transfer Acceleration

Use when:

* Uploading from users around the world
* Large files
* Need fastest upload

Usually combined with:

✅ Multipart Upload

---

# Multipart Upload

Split a huge file into pieces and upload simultaneously.

Good for:

* Large files (>100 MB, especially GBs)

---

# Cross-Region Replication (CRR)

Copies objects to another AWS Region.

Used for:

* Disaster Recovery
* Compliance
* Global redundancy

Not used for fastest uploads.

---

# Prefix

A prefix is simply a folder-like path.

Example:

```
bucket
│
├── finance/
├── logs/
├── images/
```

Lifecycle rules can target only one prefix.

Example:

```
finance/*
↓

One Zone-IA

Everything else
↓

Glacier
```

---

# Intelligent-Tiering

AWS automatically moves objects between access tiers based on usage.

Use when:

* You don't know future access patterns.

---

# Frequently Tested Comparisons

| Requirement                             | Answer                 |
| --------------------------------------- | ---------------------- |
| Frequently accessed                     | Standard               |
| Rarely accessed but immediate retrieval | Standard-IA            |
| Rarely accessed + easily reproducible   | One Zone-IA            |
| Unknown access pattern                  | Intelligent-Tiering    |
| Archive                                 | Glacier                |
| Automatically move objects              | Lifecycle Policy       |
| Recover deleted objects                 | Versioning             |
| Protect permanent deletion              | MFA Delete             |
| Encrypt before AWS                      | Client-side Encryption |
| AWS-managed encryption                  | SSE-S3                 |
| KMS encryption                          | SSE-KMS                |
| Fast global downloads                   | CloudFront             |
| Fast global uploads                     | Transfer Acceleration  |
| Large uploads                           | Multipart Upload       |

---

# One-Minute Exam Memory

* **Standard** → Frequent
* **Standard-IA** → Rare + Immediate
* **One Zone-IA** → Rare + Cheap + Reproducible
* **Intelligent-Tiering** → Unknown access
* **Glacier** → Archive
* **Lifecycle** → Auto move
* **Versioning** → Recover delete/overwrite
* **MFA Delete** → Extra delete protection
* **CloudFront** → Fast downloads
* **Transfer Acceleration** → Fast uploads
* **Multipart Upload** → Large files
* **Client-side Encryption** → AWS never sees plaintext or your master key

---

## Next: Amazon SQS (Simple Queue Service)

As you start SQS, these are the core ideas you'll see repeatedly:

1. **Standard Queue vs FIFO Queue**

   * Standard: highest throughput, *at least once* delivery, best-effort ordering.
   * FIFO: exactly-once processing (within FIFO guarantees) and strict ordering.

2. **Visibility Timeout**

   * A message isn't deleted when a consumer receives it.
   * It becomes temporarily invisible while being processed.
   * Delete it after successful processing.

3. **Long Polling vs Short Polling**

   * Long polling reduces empty responses and lowers cost.

4. **Dead-Letter Queue (DLQ)**

   * Messages that repeatedly fail processing are moved here for troubleshooting.

5. **SQS is for decoupling applications**

   * Producer → SQS → Consumer
   * Producers and consumers don't need to run at the same speed.

We'll go through each concept with questions just like we did for Lambda, RDS, and S3.


24-July-2026
