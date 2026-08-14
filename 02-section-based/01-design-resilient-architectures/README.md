Absolutely. And **you are not late**. In fact, this is a good point to pause and consolidate before continuing with High-Performing Architectures.

You have already covered a lot of Resilient Architecture questions, so instead of summarizing every question individually, I'll give you the **SAA exam mental model**.

# AWS SAA — Design Resilient Architectures Summary

Think of **resilience** as:

> **"If something fails, how does my application continue working?"**

The major concepts you encountered can be grouped into **7 buckets**:

---

## 1. High Availability

### Multi-AZ

The most important SAA concept.

```text
                Load Balancer
                 /        \
                /          \
           AZ-1             AZ-2
          EC2              EC2
           │                │
           └──────┬─────────┘
                  │
               Database
              Multi-AZ
```

If one Availability Zone fails, the application can continue operating from another AZ.

### Remember:

**AZ failure → Multi-AZ**

Examples:

* EC2 + Auto Scaling across AZs
* RDS Multi-AZ
* ALB across multiple AZs

---

# 2. RDS Multi-AZ

Your Question 20 tested this.

When the primary RDS instance fails:

```text
Before:

Primary ───────────► Application
   │
   │ replication
   ▼
Standby
```

After failure:

```text
Standby
   │
   └────► becomes Primary
```

AWS changes the **CNAME** to point to the new primary.

### Memorize:

> **RDS Multi-AZ = High Availability + automatic failover**

Not primarily for:

❌ Read scaling

For read scaling:

> **Read Replicas**

So:

| Requirement                     | Solution                                                 |
| ------------------------------- | -------------------------------------------------------- |
| High availability               | Multi-AZ                                                 |
| Automatic failover              | Multi-AZ                                                 |
| Read scaling                    | Read Replica                                             |
| Disaster recovery across Region | Cross-Region Read Replica / backups depending on service |

---

# 3. Auto Scaling

Auto Scaling protects you against changing demand and instance failure.

Example:

```text
Traffic increases
       ↓
ASG launches instances
       ↓
More capacity
```

Traffic decreases:

```text
Traffic decreases
       ↓
ASG terminates instances
       ↓
Lower cost
```

And if an EC2 instance becomes unhealthy:

```text
Unhealthy EC2
      ↓
ASG replaces it
```

### Memorize:

> **ASG = automatically maintain the desired number of healthy EC2 instances**

And deploy ASG across multiple AZs for resilience.

---

# 4. Lambda Resilience / Limits

Your Question 17 tested a very important exam fact.

Lambda maximum execution time:

> **15 minutes = 900 seconds**

So:

```text
Lambda starts
     ↓
15 minutes
     ↓
AWS terminates execution
```

If the workload takes longer than 15 minutes:

❌ Lambda may not be appropriate.

Think about:

* ECS
* AWS Batch
* Step Functions + smaller Lambda functions
* EC2
* other suitable compute options

### Memorize:

> **Lambda = maximum 15 minutes per invocation**

Also remember:

**Concurrency limit reached → throttling**, not simply "the current invocation gets killed because concurrency was exceeded."

---

# 5. Storage Resilience

You saw several storage questions.

The easiest way to remember them is:

### S3

```text
Object storage
```

Use for:

* files
* images
* videos
* backups
* data lakes
* static assets

Think:

> **S3 = massive durable object storage**

---

### EBS

```text
Block storage
      ↓
Usually attached to EC2
```

Think:

> **EBS = disk for EC2**

It is generally associated with an individual EC2 instance rather than being a shared file system for thousands of instances.

---

### EFS

```text
File system
     ↓
Many Linux EC2 instances
     ↓
NFS
```

Think:

> **EFS = shared Linux file system**

Your Question 6 is a classic:

> 1000 Linux servers + NFS + shared files

Immediately think:

**EFS**

---

### FSx for Windows File Server

Think:

> **Windows + SMB + Active Directory + NTFS**

Your earlier question:

```text
Windows
SMB
AD
NTFS
       ↓
FSx for Windows File Server
```

---

### FSx for Lustre

Think:

> **HPC + high-performance computing + compute-intensive workloads**

---

# 6. Disaster Recovery

This is another major part of resilience.

You need to distinguish:

### Same AZ

Not resilient enough.

### Multi-AZ

Protects against:

> **Availability Zone failure**

### Multi-Region

Protects against:

> **Region failure**

Your Redshift Question 22 tested this.

If the entire AWS Region disappears:

```text
Region A
   ↓
Redshift
   ↓
Cross-Region Snapshot
   ↓
Region B
```

### Memorize:

> **AZ failure → Multi-AZ**

> **Region failure → Cross-Region strategy**

---

# 7. Messaging & Event-Driven Resilience

This appeared repeatedly.

## SQS

Think:

> **Queue = buffer + decoupling**

Example:

```text
Producer
   ↓
 SQS
   ↓
Consumers
```

If consumers become temporarily unavailable, messages can remain in the queue.

This helps prevent one component's failure from immediately breaking another component.

---

## SQS priority

Your Question 2 was important.

SQS doesn't give individual messages a simple:

```text
priority = HIGH
```

Instead:

```text
Premium Queue ──────► Workers
                         ↑
Free Queue ──────────────┘
```

Workers poll the premium queue first.

So remember:

> **Different priorities → separate SQS queues**

---

# 8. DynamoDB Streams

Your Question 4 tested this.

When DynamoDB data changes:

```text
DynamoDB
   ↓
DynamoDB Streams
   ↓
Lambda
```

Think:

> **"Something changed in DynamoDB → DynamoDB Streams"**

Not CloudWatch Alarm.

CloudWatch monitors **metrics**.

DynamoDB Streams captures **data changes**.

---

# 9. Kinesis

Your Question 5:

GPS coordinates every 5 seconds from millions of trucks.

That's:

> **Real-time streaming data**

Think:

```text
GPS
 ↓
Kinesis
 ↓
Multiple consumers
 ↓
Analytics
```

### Memorize:

> **Kinesis = streaming data**

Examples:

* IoT telemetry
* GPS
* clickstreams
* logs
* real-time analytics

---

# 10. CloudWatch + SNS

Your Question 25:

> Monitor database metrics and send email notifications.

Classic architecture:

```text
Database
   ↓
CloudWatch
   ↓
Alarm
   ↓
SNS
   ↓
Email
```

Memorize:

> **CloudWatch = monitor**

> **SNS = notify**

Very common SAA pattern.

---

# 11. Security Groups & Least Privilege

Your Question 23 is extremely important.

Suppose:

```text
ALB
 ↓
Application EC2
 ↓
Database
```

Don't do:

```text
Database SG
Source: 0.0.0.0/0
Port: 1433
```

Instead:

```text
Database SG

Inbound:
Port 1433
Source: Application-SG
```

So:

> **Security Group → Security Group reference**

This is especially useful when EC2 instances are managed by an Auto Scaling Group.

Why?

Because instances come and go.

You don't want:

```text
Instance A
Instance B
Instance C
```

hardcoded into the database security group.

Instead:

```text
Application SG
       ↓
Database SG
```

New instances automatically benefit from the rule.

### Exam memory:

> **Dynamic EC2 fleet → reference the Security Group, not individual IPs/instances**

---

# 12. Internet Connectivity

Your Question 26 tested this.

For an EC2 instance to be directly reachable from the Internet, you generally need:

### 1. Public IPv4/EIP

and

### 2. Route to Internet Gateway

```text
Internet
   ↓
Internet Gateway
   ↓
Route Table
   ↓
Public Subnet
   ↓
EC2
```

So memorize:

> **Public subnet = route to IGW**

And:

> **EC2 needs a public IP/EIP for direct Internet reachability**

---

# 13. Transit Gateway

Your Question 27.

Suppose:

```text
VPC HR
   \
    \
     Transit Gateway
    /
   /
VPC Finance
```

Transit Gateway is the **network hub**.

For multiple VPCs:

> **Transit Gateway = central network hub**

And if VPCs are in different Regions:

> **Transit Gateway + inter-Region peering**

---

# 14. Network Firewall

Your Question 27 also tested this.

If the requirement says:

> **IPS / inspect traffic / block exploits**

Think:

### AWS Network Firewall

It provides network traffic inspection and IPS capabilities.

Don't confuse it with:

### Security Hub

Security Hub:

> Security findings / security posture

It isn't your inline network traffic firewall.

---

# 15. AWS Organizations + SCP

Your Question 29.

When you have:

```text
AWS Organization
 ├── Finance Account
 ├── HR Account
 ├── Engineering Account
 └── Security Account
```

And the requirement is:

> Centrally control which AWS services/actions accounts can use.

Think:

### AWS Organizations + SCP

Important distinction:

**SCP does NOT grant permissions.**

It sets the **maximum available permissions / guardrails** for accounts.

Think:

> **SCP = organizational guardrail**

---

# 16. Redshift Disaster Recovery

Your Question 22:

If you need protection against a **Region outage**:

> **Redshift Cross-Region Snapshot Copy**

Don't think:

```text
Automated snapshot only
```

because the snapshot is still in the affected Region.

Think:

```text
Region A
Redshift
   ↓
Cross-Region Snapshot
   ↓
Region B
```

---

# 17. S3 Glacier Retrieval

Your Question 30.

Three important retrieval concepts from your question:

| Retrieval | Think            |
| --------- | ---------------- |
| Expedited | Minutes          |
| Standard  | Hours            |
| Bulk      | Cheapest / hours |

If the question says:

> **Need data within minutes + predictable retrieval capacity**

Think:

**Expedited + Provisioned Retrieval Capacity**

The key phrase is:

> **"under all circumstances"**

That's what pushes you toward **Provisioned Capacity**.

---

# 18. Resilient Architecture — Your Mental Framework

When you get a resilience question, ask these questions in order:

### Question 1

**What can fail?**

* EC2?
* AZ?
* Region?
* Database?
* Network?
* Application component?

### Question 2

**What happens when it fails?**

Do I need:

* replacement?
* failover?
* replication?
* backup?
* queue?
* multi-AZ?
* multi-Region?

### Question 3

**Does the application continue automatically?**

Look for:

* Auto Scaling
* Multi-AZ
* failover
* replication
* managed services
* queues
* event-driven architecture

### Question 4

**Is the solution cost-effective?**

This is where questions like your EC2 Reserved + Spot example come in.

---

# ⭐ The Resilient Architecture Cheat Sheet

This is what I'd memorize before moving forward:

| Requirement / Keyword          | Think                                               |
| ------------------------------ | --------------------------------------------------- |
| EC2 failure                    | Auto Scaling                                        |
| AZ failure                     | Multi-AZ                                            |
| Region failure                 | Multi-Region / cross-region replication             |
| RDS HA                         | Multi-AZ                                            |
| RDS read scaling               | Read Replicas                                       |
| Lambda maximum execution       | **15 minutes**                                      |
| Shared Linux filesystem        | EFS                                                 |
| Windows + SMB + AD             | FSx Windows                                         |
| HPC filesystem                 | FSx Lustre                                          |
| Object storage                 | S3                                                  |
| EC2 disk                       | EBS                                                 |
| Queue / decoupling             | SQS                                                 |
| Real-time stream               | Kinesis                                             |
| DynamoDB data changes          | DynamoDB Streams                                    |
| Monitoring                     | CloudWatch                                          |
| Notifications                  | SNS                                                 |
| Multiple VPCs                  | Transit Gateway                                     |
| Network IPS                    | Network Firewall                                    |
| Organization-wide restrictions | Organizations + SCP                                 |
| Private EC2 administration     | Bastion / Session Manager depending on requirements |
| Dynamic EC2 → DB access        | Security Group reference                            |
| Public EC2                     | Public IP + route to IGW                            |
| Redshift Region DR             | Cross-Region Snapshot Copy                          |
| Glacier urgent retrieval       | Expedited                                           |
| Guaranteed expedited capacity  | Provisioned Retrieval Capacity                      |

---

## One final distinction I really want you to remember

**Resilient ≠ High Performing ≠ Secure ≠ Cost Optimized**

They overlap, but the exam is asking different questions.

### Resilient

> **"What happens when something breaks?"**

Multi-AZ, failover, backups, replication, Auto Scaling, queues.

### High Performing

> **"How do I make it faster and handle more workload?"**

Caching, CloudFront, Kinesis, EFS performance, instance types, read replicas, DynamoDB scaling, etc.

### Secure

> **"How do I prevent unauthorized access or protect data?"**

IAM, KMS, Security Groups, Network Firewall, SCP, WAF, encryption, least privilege.

### Cost Optimized

> **"How do I achieve the requirement for the lowest reasonable cost?"**

Spot, Reserved/Savings Plans, right-sizing, lifecycle policies, serverless, S3 storage classes, etc.

**Your next chapter—High-Performing Architectures—should therefore be studied with a different mental question:**

> 🚀 **"How can I make this architecture faster, more scalable, and able to handle higher workloads?"**

And yes, from here onward, **I can summarize each chapter for you before we move to the next one**, so you don't end up with the same gap again.

14-August-2026
