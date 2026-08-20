# Your S3 Storage Class Cheat Sheet

| Storage class                     | Access           | Retrieval         | AZs      | Main use                             |
| --------------------------------- | ---------------- | ----------------- | -------- | ------------------------------------ |
| **S3 Standard**                   | Frequent         | Milliseconds      | Multiple | Active data                          |
| **S3 Standard-IA**                | Infrequent       | Milliseconds      | Multiple | Important infrequently accessed data |
| **S3 One Zone-IA**                | Infrequent       | Milliseconds      | **One**  | Reproducible/non-critical data       |
| **S3 Intelligent-Tiering**        | Unknown/changing | Automatic tiering | Multiple | Unpredictable access                 |
| **S3 Glacier Instant Retrieval**  | Rare             | **Milliseconds**  | Multiple | Archive + immediate retrieval        |
| **S3 Glacier Flexible Retrieval** | Rare             | Minutes–hours     | Multiple | Archive where retrieval can wait     |
| **S3 Glacier Deep Archive**       | Very rare        | Hours             | Multiple | Long-term archival                   |

### One very important correction to your mental model

Don't memorize:

> "Glacier = slow."

That's **too broad**.

There are different Glacier classes:

**Glacier Instant Retrieval** → milliseconds
**Glacier Flexible Retrieval** → minutes to hours
**Glacier Deep Archive** → hours

So always look at the **specific Glacier class**.

---

## 🧠 SAA decision tree

When you get an S3 storage question, ask these in order:

**1. Frequently accessed?**

→ **S3 Standard**

**2. Infrequently accessed but immediate/millisecond retrieval?**

→ **Standard-IA**

**3. Same as above, but data is reproducible and single-AZ is acceptable?**

→ **One Zone-IA**

**4. Access pattern unknown/unpredictable?**

→ **Intelligent-Tiering**

**5. Archive but millisecond retrieval required?**

→ **Glacier Instant Retrieval**

**6. Archive and can wait minutes/hours?**

→ **Glacier Flexible Retrieval**

**7. Very long-term archive and rarely accessed?**

→ **Glacier Deep Archive**

That decision tree is much more useful for SAA than simply memorizing the names.


19-August-2026



### 🧠 Your EC2 shortcut

| Requirement in question                 | Think                                   |
| --------------------------------------- | --------------------------------------- |
| Can be interrupted                      | 🟢 **Spot**                             |
| Non-production / batch processing       | 🟢 **Spot**                             |
| Need guaranteed capacity in specific AZ | 🔵 **Capacity Reservation**             |
| Predictable 1–3 year workload           | 🟡 **Reserved Instance / Savings Plan** |
| Short-term / unpredictable              | 🟠 **On-Demand**                        |
| Entire physical server / licensing      | 🔴 **Dedicated Host**                   |


Absolutely. Before we move to review mode, let's build a **clean mental model** for the entire **Design Cost-Optimized Architectures** section.

And yes — I think your biggest gap isn't actually individual AWS services. It's **knowing which service to choose when the question describes a data movement / connectivity problem**. So I'll organize this around decision-making rather than memorization.

# CSAA — Design Cost-Optimized Architectures: Final Summary

## 1. The biggest rule

For almost every question, ask:

> **What is the workload, how long will I use it, can it be interrupted, and how much data am I moving?**

Those four questions solve a huge percentage of cost-optimization questions.

---

# 2. EC2 purchasing options

This is one of the most important areas.

| Option                   | Mental model                                    | Use when                                            |
| ------------------------ | ----------------------------------------------- | --------------------------------------------------- |
| **On-Demand**            | "I need it now, no commitment"                  | Short/unpredictable workloads                       |
| **Spot**                 | "I want it cheap and can tolerate interruption" | Batch, non-critical, flexible workloads             |
| **Reserved Instances**   | "I know I'll use this for a long time"          | Predictable steady workloads                        |
| **Savings Plans**        | "I commit to compute spend"                     | Long-term predictable compute                       |
| **Capacity Reservation** | "I MUST have capacity available in this AZ"     | Critical workloads requiring guaranteed capacity    |
| **Dedicated Host**       | "I need an entire physical server"              | Licensing/compliance/specific hardware requirements |

### Exam shortcuts

**Can be interrupted?**

→ **Spot**

**Predictable for 1–3 years?**

→ **Reserved / Savings Plan**

**Must guarantee capacity in a specific AZ?**

→ **Capacity Reservation**

**Short-term / unpredictable?**

→ **On-Demand**

**Need physical host?**

→ **Dedicated Host**

---

# 3. EC2 states and billing

Remember:

```text
pending
   ↓
running
   ↓
stopping → stopped
   ↓
shutting-down
   ↓
terminated
```

The important exam points:

### Running

💰 Compute charges.

### Stopped

No EC2 compute charge for On-Demand instance.

But:

💰 EBS storage still costs money.

### Terminated

EC2 instance is gone.

But if you purchased a **Reserved Instance**, the commitment doesn't disappear.

You can still be paying for the reservation.

### Hibernation

Different from ordinary stopping.

Hibernation saves the RAM state to EBS so the instance can resume faster.

During hibernation:

* no normal EC2 running compute charge
* EBS still costs
* attached Elastic IP may cost depending on circumstances

---

# 4. Auto Scaling

If the application has variable demand:

### Target Tracking

Think:

> **"Keep CPU around 50%."**

AWS automatically adds/removes instances to maintain the target.

Excellent for:

> avoiding over-provisioning while maintaining performance.

### Scheduled Scaling

Think:

> **"Every day at 10 AM traffic increases."**

Use when demand is predictable.

### Simple/Step Scaling

Uses CloudWatch alarms and scaling adjustments.

For your exam:

> **Cost optimization + automatically match capacity to demand → Target Tracking**

---

# 5. S3 Storage Classes

This was one of your weaker areas, so this is worth memorizing.

Think in terms of **access frequency + retention + retrieval speed + durability requirements**.

| Class                          | Mental model                               |
| ------------------------------ | ------------------------------------------ |
| **S3 Standard**                | Frequently accessed / short-lived          |
| **S3 Intelligent-Tiering**     | Access pattern unknown or changing         |
| **S3 Standard-IA**             | Infrequent access but needs fast retrieval |
| **S3 One Zone-IA**             | Infrequent + reproducible data             |
| **Glacier Instant Retrieval**  | Archive + millisecond retrieval            |
| **Glacier Flexible Retrieval** | Archive + retrieval can take minutes/hours |
| **Glacier Deep Archive**       | Very long-term archive                     |

### Critical exam concept: minimum storage duration

Don't automatically choose Glacier because:

> "Glacier is cheaper."

Instead ask:

> **How long will the data actually stay there?**

For example:

**Logs kept for 12 hours**

→ S3 Standard.

Why?

Because IA/Glacier classes can have minimum-duration charges.

---

# 6. S3 Lifecycle

Lifecycle policies automate:

### Transition

```text
S3 Standard
     ↓ 30 days
Standard-IA
     ↓
Glacier
```

### Expiration

```text
S3
 ↓ 30 days
DELETE
```

So:

> **"Keep logs for one month, then delete automatically."**

→ **S3 Lifecycle expiration rule**

Not IAM.

Not bucket policy.

Not CORS.

---

# 7. S3 + CloudFront

If you have:

> Durable storage + global distribution

Think:

```text
       Users worldwide
       /      |      \
      ↓       ↓       ↓
   CloudFront Edge Locations
             ↓
            S3
```

**S3 = storage**

**CloudFront = CDN**

Don't confuse CloudFront with ElastiCache.

---

# 8. Encryption

For:

> S3 + envelope encryption + automatic key management/rotation + audit trail

Think:

**SSE-KMS**

```text
Application
     ↓
    S3
     ↓
  SSE-KMS
     ↓
   AWS KMS
     ↓
CloudTrail audit
```

### Quick comparison

| Encryption  | Key managed by | Audit/control            |
| ----------- | -------------- | ------------------------ |
| **SSE-S3**  | AWS/S3         | Basic                    |
| **SSE-KMS** | AWS KMS        | ⭐ Strong control + audit |
| **SSE-C**   | Customer       | Customer supplies key    |

If the question specifically mentions:

> **audit key usage**

→ **KMS**

---

# 9. Spot Instances

Your Q20 and Q28 are classic examples.

If:

* batch workload
* non-critical
* temporary
* can tolerate interruption

→ **Spot**

Example:

```text
1000 videos waiting
      ↓
Need extra EC2 capacity
      ↓
Spot Instances
      ↓
Backlog decreases
      ↓
Terminate instances
```

Spot can be interrupted, so your application must tolerate that.

---

# 10. Now your BIG confusion: DataSync vs Storage Gateway vs Direct Connect

This is where I want you to change your thinking.

These services are **not competitors doing the same thing**.

They solve different problems.

---

## AWS DataSync

Think:

> 🚚 **"Move my data."**

That's it.

DataSync is primarily a **data transfer/migration service**.

Example:

```text
ON-PREMISES
   │
   │ DataSync
   ↓
AWS
   │
   └── S3 / EFS / FSx
```

### Use DataSync when:

> "I have 100 TB on-premises and want to move it to S3."

or:

> "I need to continuously transfer files from on-premises to AWS."

It automates things like:

* transfer
* scheduling
* monitoring
* validation
* optimization

### Your Q4

Historical records:

```text
On-premises
     ↓
  DataSync
     ↓
S3 Glacier Deep Archive
```

Perfect.

### Mental shortcut

> **DataSync = MOVE DATA**

---

# 11. AWS Storage Gateway

This one is different.

Think:

> 🏢 **"I want my on-premises environment to work with AWS storage."**

It's a **hybrid storage service**.

For example:

```text
On-premises applications
        ↓
 Storage Gateway
        ↓
       AWS
```

The on-premises applications can continue using familiar storage protocols while AWS provides the backend storage.

### Three important Gateway types

#### File Gateway

```text
On-premises applications
        ↓
      NFS/SMB
        ↓
    File Gateway
        ↓
       S3
```

Think:

> **File access → S3**

---

#### Volume Gateway

Think:

> **Block storage → AWS**

It provides block storage volumes to on-premises applications and stores backups/snapshots in AWS.

---

#### Tape Gateway

This is especially important for your Q22.

Think:

> **"We currently use physical tapes, but want cloud backup."**

```text
Existing backup software
        ↓
    Tape Gateway
        ↓
Virtual Tape
        ↓
       S3
        ↓
Glacier
```

So Q22:

> "We currently use tape backup and need 10-year retention."

→ **Storage Gateway Tape Gateway + Glacier Deep Archive**

### Mental shortcut

> **Storage Gateway = HYBRID STORAGE**

---

# 12. DataSync vs Storage Gateway

This distinction should now be crystal clear.

### DataSync

> **Move existing data**

```text
On-premises ────→ AWS
```

### Storage Gateway

> **Connect on-premises storage/applications with AWS storage**

```text
On-premises application
        ↕
 Storage Gateway
        ↕
       AWS
```

### Easy analogy

Imagine you're moving house.

**DataSync:**

> "Move all my furniture from House A to House B."

**Storage Gateway:**

> "Keep living in House A, but use a storage facility in AWS as part of my storage system."

---

# 13. AWS Direct Connect

Now another different service.

Think:

> 🔌 **"I need a dedicated private network connection to AWS."**

```text
Company Data Center
        │
        │ Direct Connect
        │
        ↓
       AWS
```

It's primarily about **network connectivity**, not data migration.

### Use it when:

> "Our company permanently needs private, predictable connectivity between on-premises and AWS."

For example:

* hybrid architecture
* corporate applications
* Active Directory
* DNS
* databases
* private connectivity

---

# 14. Direct Connect Gateway

This is where your Q5 comes in.

Think:

> **Direct Connect Gateway = connect one Direct Connect setup to multiple VPCs across accounts/Regions (within supported architecture).**

Instead of:

```text
On-prem
  ↓
DX → VPC A

On-prem
  ↓
DX → VPC B

On-prem
  ↓
DX → VPC C
```

you can build a centralized architecture using:

```text
                 VPC A
                   ↑
                   |
On-prem → DX → DX Gateway → Transit Gateway
                   |
                   ↑
                 VPC B
```

---

# 15. Transit Gateway

This is your **network hub**.

Think:

> 🚦 **"I have many VPCs and networks. Give me one central router."**

```text
             VPC A
               |
               |
VPC B ─── Transit Gateway ─── VPC C
               |
               |
        On-Premises
```

Instead of creating tons of individual connections:

```text
VPC A ↔ VPC B
VPC A ↔ VPC C
VPC B ↔ VPC C
VPC A ↔ On-prem
VPC B ↔ On-prem
VPC C ↔ On-prem
```

You centralize routing.

### Mental shortcut

> **Transit Gateway = NETWORK HUB**

---

# 16. VPC Peering

VPC Peering is much simpler.

Think:

> **"Connect these two VPCs directly."**

```text
VPC A ←──── Peering ────→ VPC B
```

Good for simple direct connectivity.

But:

### No transitive routing

If:

```text
VPC A ←→ VPC B ←→ VPC C
```

That does **NOT** mean:

```text
VPC A → VPC C
```

through B.

This is why Transit Gateway is useful for many VPCs.

### Shortcut

> **VPC Peering = direct VPC-to-VPC**

> **Transit Gateway = central hub for many networks**

---

# 17. VPN CloudHub

Think:

> **"Connect multiple remote sites using VPN through AWS."**

For example:

```text
Office A ──VPN──┐
                ↓
              AWS
                ↑
Office B ──VPN──┘
```

It can provide connectivity between multiple remote networks.

But compared with Direct Connect:

**VPN = Internet-based**

**Direct Connect = dedicated private connection**

So in your Q5:

> Existing dedicated DX + new AWS accounts need consistent access to on-prem DNS/AD

→ **Transit Gateway + Direct Connect Gateway**

Not VPN CloudHub.

---

# 18. Data Transfer Terminal

Now your Q30.

Think:

> 🚚 **"I have a gigantic amount of data and my network is too slow."**

Example:

```text
250 TB
+
100 Mbps Internet
+
one-time migration
        ↓
❌ Don't spend weeks/months uploading
        ↓
Data Transfer Terminal
        ↓
S3
```

The key is that you **physically bring the storage to a facility with high-speed AWS connectivity**.

### Shortcut

> **Massive one-time physical dataset + network too slow → Data Transfer Terminal**

---

# 19. Put all these services together

This is the table I want you to memorize:

| Service                      | One-line meaning                                    |
| ---------------------------- | --------------------------------------------------- |
| **DataSync**                 | 🚚 Move data                                        |
| **Storage Gateway**          | 🏢 Hybrid storage                                   |
| **Direct Connect**           | 🔌 Dedicated private network                        |
| **Direct Connect Gateway**   | 🔌 Connect DX to multiple VPC/network architectures |
| **Transit Gateway**          | 🚦 Central network hub                              |
| **VPC Peering**              | 🔗 Direct VPC-to-VPC connection                     |
| **VPN CloudHub**             | 🌐 Connect multiple sites using VPN                 |
| **Data Transfer Terminal**   | 🚛 Physically transfer huge datasets                |
| **S3 Transfer Acceleration** | ⚡ Accelerate Internet uploads to S3                 |

---

# 20. The Cost Optimization decision tree

When you see a question, run this in your head:

### Compute

```text
Can it be interrupted?
      ↓
    YES → Spot

No interruption + long predictable use?
      ↓
Reserved / Savings Plan

Must guarantee AZ capacity?
      ↓
Capacity Reservation

Short/unpredictable?
      ↓
On-Demand
```

### Storage

```text
Frequently accessed?
      ↓
S3 Standard

Unknown/changing access?
      ↓
Intelligent-Tiering

Infrequent + quick access?
      ↓
Standard-IA

Infrequent + reproducible?
      ↓
One Zone-IA

Archive?
      ↓
Glacier
```

Then ask:

> How quickly must I retrieve it?

```text
Milliseconds → Glacier Instant Retrieval
Minutes/hours acceptable → Flexible Retrieval
Very long-term archive → Deep Archive
```

### Data movement

```text
Need to MOVE data?
      ↓
   DataSync

Need HYBRID storage?
      ↓
Storage Gateway

Need PRIVATE dedicated connectivity?
      ↓
Direct Connect

Many VPCs/networks?
      ↓
Transit Gateway

Two VPCs directly?
      ↓
VPC Peering

Huge physical dataset + network too slow?
      ↓
Data Transfer Terminal
```

---

# 🎯 Your biggest lessons from Questions 4–30

I'd say you should especially remember these **10 triggers**:

1. **Can be interrupted → Spot**
2. **Guaranteed AZ capacity → Capacity Reservation**
3. **Long predictable EC2 usage → Reserved/Savings Plan**
4. **Temporary S3 data → Standard**
5. **Reproducible + infrequent → One Zone-IA**
6. **Long-term archive → Glacier/Deep Archive**
7. **Automatic S3 transition/delete → Lifecycle**
8. **S3 global delivery → CloudFront**
9. **Move on-prem data → DataSync**
10. **Hybrid storage → Storage Gateway**
11. **Dedicated private network → Direct Connect**
12. **Many VPCs → Transit Gateway**
13. **Two VPCs → VPC Peering**
14. **Massive one-time physical migration → Data Transfer Terminal**

You don't need to memorize the explanations word-for-word. **Memorize the trigger → service relationship.**

And yes, **we can now move to review mode**. In review mode, I'll focus much more on **why you got an answer wrong, the concept behind it, and the exam trigger**, rather than re-teaching every paragraph of the TutorialsDojo explanation.

20-August-2026
