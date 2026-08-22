



### Exam shortcut 🧠

Remember this pair:

| Problem                              | Think of                       |
| ------------------------------------ | ------------------------------ |
| Need logic closer to global users    | **Lambda@Edge**                |
| Origin sometimes fails               | **CloudFront Origin Failover** |
| Static content slow                  | **CloudFront caching**         |



22-August-2026


<details>
  <summary>Old</summary>
  



Absolutely. Here is the **one-page CloudFront summary** that covers the important concepts and the patterns from all 8 questions. If you remember this, you'll answer most CloudFront questions in SAA.

---

# Amazon CloudFront Summary (SAA-C03)

## What is CloudFront?

CloudFront is AWS's **Content Delivery Network (CDN)**.

Instead of every user downloading content from your origin server, CloudFront caches content in AWS Edge Locations around the world.

```
User
   │
Nearest Edge Location
   │
CloudFront
   │
Origin (S3 / ALB / EC2)
```

Benefits:

* Faster loading
* Lower latency
* Less load on origin
* Lower bandwidth costs
* Better scalability

---

# CloudFront Origins

CloudFront can fetch content from:

* S3 Bucket ✅
* EC2 ✅
* Application Load Balancer (ALB) ✅
* Custom HTTP Server ✅

Exam favorite:

```
Users
     ↓
CloudFront
     ↓
ALB
     ↓
EC2
```

or

```
Users
     ↓
CloudFront
     ↓
S3
```

---

# Question 1 — DDoS Protection

### Easy wording

"My website is under attack."

Need:

* Block attackers
* Reduce DDoS
* Protect application

Answer:

```
CloudFront

+

AWS WAF (Rate-based Rules)

+

ALB
```

Remember:

CloudFront = first line of defense

AWS WAF = filters malicious requests

AWS Shield Standard = automatic protection

Keywords:

* DDoS
* HTTP Flood
* Rate limit
* Web ACL

→ Think **CloudFront + WAF**

---

# Question 2 — Multiple Private Files

Easy wording:

"I have premium members."

Need:

* Access many files
* Don't change URLs

Answer:

Signed Cookies

Memory:

| Need       | Use            |
| ---------- | -------------- |
| One file   | Signed URL     |
| Many files | Signed Cookies |

Keywords:

* Multiple files
* Subscribers
* Existing URLs

→ Signed Cookies

---

# Question 3 — Secure S3 Access

Easy wording:

"I don't want users accessing S3 directly."

Need:

```
Users

↓

CloudFront

↓

S3
```

Not

```
Users

↓

S3
```

Answer:

* Origin Access Control (OAC)
* Signed URLs / Signed Cookies

Memory:

OAC locks S3.

CloudFront becomes the only way to reach S3.

---

# Question 4 — Hotlinking

Easy wording:

Other websites are stealing my images.

Current:

```
Website A

↓

Image from YOUR S3
```

Everyone is using your bandwidth.

Referrer headers were spoofed.

Answer:

Private Bucket

*

Pre-Signed URLs

Why?

Only users with temporary URLs can download.

Keywords:

* Hotlinking
* Referrer spoofed
* Temporary access

→ Pre-Signed URL

---

# Question 5 — Static Files Worldwide

Need:

* Static files
* Global users
* Cheap
* Fast

Answer:

```
CloudFront

↓

S3
```

Not:

* EC2
* Fargate
* Lambda
* Global Accelerator

Keywords:

* Static
* Worldwide
* Low latency
* Cheap

---

# Question 6 — Slow Website

Need:

Website is slow.

Readers leave.

Need faster reads.

Answer:

CloudFront

*

ElastiCache

Difference:

CloudFront caches static/web content close to users.

ElastiCache caches database/application data in memory.

Memory:

CloudFront = Internet cache

ElastiCache = Database cache

---

# Question 7 — Cache Misses

Problem:

Every request reaches origin.

CloudFront never serves cached objects.

Answer:

```
Cache-Control

max-age=0
```

means

```
Never keep cache.
```

Every request:

```
User

↓

CloudFront

↓

Origin

↓

CloudFront

↓

User
```

instead of

```
User

↓

CloudFront Cache

↓

User
```

Keywords:

* Origin always hit
* No caching
* max-age = 0

---

# Question 8 — Cheapest Static Website

Need:

* HTML
* CSS
* JavaScript
* Images

Answer:

```
S3 Website Hosting

↓

CloudFront
```

No EC2.

No Beanstalk.

No Auto Scaling.

S3 already scales automatically.

---

# CloudFront Decision Table

| Requirement            | AWS Service        |
| ---------------------- | ------------------ |
| Global CDN             | CloudFront         |
| Static Website         | S3 Website Hosting |
| Faster Downloads       | CloudFront         |
| Cache Static Files     | CloudFront         |
| Cache Database Queries | ElastiCache        |
| Private S3 Access      | OAC                |
| Single Private File    | Signed URL         |
| Multiple Private Files | Signed Cookies     |
| Block DDoS             | WAF + CloudFront   |
| Temporary S3 Download  | Pre-Signed URL     |
| Global TCP/UDP Apps    | Global Accelerator |

---

# Services Often Confused

## CloudFront vs Global Accelerator

| CloudFront     | Global Accelerator                |
| -------------- | --------------------------------- |
| CDN            | Network Accelerator               |
| HTTP/HTTPS     | TCP/UDP                           |
| Caches content | Doesn't cache                     |
| Static files   | Gaming, VoIP, APIs with static IP |
| S3 integration | No direct S3 origin               |

---

## Signed URL vs Signed Cookies

| Signed URL  | Signed Cookies     |
| ----------- | ------------------ |
| One file    | Multiple files     |
| URL changes | URL stays the same |
| Downloads   | Member areas       |

---

## Pre-Signed URL vs Signed URL

Many students confuse these.

### S3 Pre-Signed URL

* Generated by S3
* Temporary access to an S3 object
* Bypasses CloudFront
* Good for uploads/downloads directly to S3

### CloudFront Signed URL

* Generated for CloudFront
* User must access through CloudFront
* Used for private CDN content

---

## OAC vs Public Bucket

Without OAC:

```
Users

↓

CloudFront

↓

S3

OR

↓

Direct S3 URL
```

Bad ❌

With OAC:

```
Users

↓

CloudFront

↓

S3
```

Direct S3 access is blocked. ✅

---

# High-Frequency Exam Keywords

| If you read...         | Think...                  |
| ---------------------- | ------------------------- |
| Static website         | S3 Website Hosting        |
| Global users           | CloudFront                |
| CDN                    | CloudFront                |
| Faster downloads       | CloudFront                |
| Low latency            | CloudFront                |
| Multiple private files | Signed Cookies            |
| One private file       | Signed URL                |
| Private S3             | OAC                       |
| DDoS                   | WAF + CloudFront          |
| Temporary S3 access    | Pre-Signed URL            |
| Origin always hit      | Cache-Control max-age = 0 |
| Read caching           | ElastiCache               |

---

## Final Memory Map

```
                    CloudFront
                         │
      ┌──────────────────┼──────────────────┐
      │                  │                  │
   Faster CDN       Secure Content      Lower Origin Load
      │                  │                  │
  Edge Locations      OAC             Cached Objects
      │                  │                  │
 Static Website     Signed URL        Cache-Control
      │                  │                  │
     S3          Signed Cookies      max-age
      │
   AWS WAF
      │
 DDoS Protection
```

This completes the **CloudFront** section. Combined with your previous summaries on **SQS** and **VPC**, you now have a solid foundation for three of the most commonly tested AWS SAA topics.


27-July-2026


</details>

