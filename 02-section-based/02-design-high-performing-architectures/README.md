
Absolutely. You have now completed a substantial set of **CSAA – Design High-Performing Architectures** questions. Before moving to Secure Architectures, let's consolidate the concepts so you don't just remember individual answers.

# AWS SAA — High-Performing Architectures Summary

Think of this domain as:

> **"How do I make my AWS application fast, scalable, and efficient?"**

---

## 1. EC2 Monitoring & Custom Metrics

### Default EC2 CloudWatch metrics

EC2 provides metrics such as:

* CPU utilization
* Network traffic
* Disk read/write activity
* Network packets

But **memory utilization is not available by default**.

For memory:

```text
EC2
 ↓
CloudWatch Agent
 ↓
Custom Memory Metric
 ↓
CloudWatch Alarm
 ↓
ASG Scaling
```

### Remember

> **CPU = default**
> **Memory = CloudWatch Agent/custom metric**

This appeared in Q3 and Q23.

---

# 2. Auto Scaling Policies

There are several important scaling mechanisms.

### Target Tracking

You give AWS a **target**.

Example:

> Keep average CPU around 50%.

```text
CPU = 70%
   ↓
ASG adds instances

CPU = 30%
   ↓
ASG removes instances
```

Think:

> **Target Tracking = thermostat**

---

### Step Scaling

You define **different scaling actions depending on how badly the alarm is breached**.

For example:

```text
CPU 50–60% → +1 instance
CPU 60–80% → +2 instances
CPU >80%   → +3 instances
```

Think:

> **Step = different steps depending on severity**

Q19 tested this.

---

### Simple Scaling

One scaling adjustment.

```text
CPU > 70%
   ↓
+2 instances
```

Less flexible than step/target tracking.

---

### Scheduled Scaling

You know demand in advance.

Example:

> Every weekday at 9 AM, increase capacity.

Think:

> **Scheduled = predictable traffic**

---

### Exam shortcut

| Situation                                      | Policy                |
| ---------------------------------------------- | --------------------- |
| Maintain CPU at 50%                            | **Target tracking**   |
| Different actions for different alarm breaches | **Step scaling**      |
| One fixed scaling action                       | **Simple scaling**    |
| Known schedule                                 | **Scheduled scaling** |

---

# 3. Storage — Extremely Important

This was one of the biggest themes in your questions.

## EBS

**Block storage**

Usually:

```text
EC2 → EBS
```

Good for:

* OS disks
* databases
* low-latency block storage
* applications requiring block-level storage

Think:

> **EBS = hard drive for EC2**

---

## EFS

**Shared file system for Linux**

Uses:

> **NFS**

Multiple EC2 instances can access it concurrently.

```text
EC2 ─┐
EC2 ─┼──► EFS
EC2 ─┘
```

Think:

> **EFS = shared Linux filesystem**

---

## FSx for Windows File Server

For:

* Windows
* SMB
* Microsoft Active Directory
* Windows file shares

Think:

> **Windows → FSx Windows**

---

## FSx for Lustre

For:

* HPC
* machine learning
* big data
* high-performance computing
* parallel processing
* very high throughput

Think:

> **Lustre = extremely fast parallel filesystem**

---

## FSx for NetApp ONTAP

Enterprise storage.

Supports:

* NFS
* SMB
* iSCSI
* file + block workloads

Can provide:

* Multi-AZ
* high performance
* enterprise storage capabilities

Think:

> **ONTAP = enterprise multiprotocol storage**

---

## FSx for OpenZFS

High-performance file storage based on OpenZFS.

Think:

> **OpenZFS = high-performance NFS/file workloads**

---

### Storage cheat sheet

| Requirement                | Service           |
| -------------------------- | ----------------- |
| EC2 block storage          | **EBS**           |
| Shared Linux filesystem    | **EFS**           |
| Windows + SMB + AD         | **FSx Windows**   |
| HPC/ML parallel filesystem | **FSx Lustre**    |
| Enterprise NFS/SMB/iSCSI   | **FSx ONTAP**     |
| High-performance OpenZFS   | **FSx OpenZFS**   |
| Object storage             | **S3**            |
| Temporary Lambda storage   | **Lambda `/tmp`** |

---

# 4. Hot vs Cold Storage

### Hot data

Frequently accessed / performance-sensitive.

Examples:

* FSx Lustre
* EBS
* EFS depending on workload

### Cold data

Rarely accessed.

Think:

> **S3 Glacier / Glacier Deep Archive**

For example:

```text
Frequently accessed ML datasets
          ↓
    FSx for Lustre

Old/rarely accessed datasets
          ↓
    S3 Glacier
```

---

# 5. Kinesis / Streaming

You learned an important distinction.

### Kinesis Data Streams

Think:

> **I need to process the stream.**

Useful for:

* real-time processing
* multiple consumers
* ordering
* replay

```text
Producers
    ↓
Kinesis Data Streams
    ↓
Consumers
```

### Amazon Data Firehose

Think:

> **I need to deliver streaming data somewhere.**

```text
Streaming data
      ↓
   Firehose
      ↓
 S3 / Redshift / OpenSearch / Splunk
```

### SQS

Think:

> **Queue messages between applications.**

### DynamoDB Streams

Think:

> **What changed in my DynamoDB table?**

### Memory trick

> **Streams = process**
> **Firehose = deliver**
> **SQS = queue**
> **DynamoDB Streams = DynamoDB changes**

---

# 6. DynamoDB + Lambda

If the requirement says:

> "When an item changes in DynamoDB, invoke Lambda."

Think:

**DynamoDB Streams → Lambda**

```text
DynamoDB
   ↓
DynamoDB Streams
   ↓
Lambda
```

Very common SAA pattern.

---

# 7. DynamoDB + AppSync

If you see:

> Millions of users + real-time application + rapidly changing schema

Think:

**DynamoDB + AppSync**

DynamoDB:

* scalable
* serverless
* NoSQL
* key-value/document
* flexible schema

AppSync:

* managed GraphQL/API layer
* real-time data synchronization

---

# 8. Load Balancers

This is another major area.

## ALB

**Application Load Balancer**

Layer 7.

Works with HTTP/HTTPS.

Can route based on:

* path
* hostname
* HTTP headers
* HTTP method
* query string
* source IP

Example:

```text
/api/android → Android Target Group

/api/ios → iOS Target Group
```

Think:

> **ALB = intelligent HTTP routing**

---

## NLB

**Network Load Balancer**

Layer 4.

Supports:

* TCP
* UDP
* TLS

Also important:

> **Static IP / Elastic IP**

Useful when clients have firewalls that whitelist specific IP addresses.

Also useful for UDP gaming applications.

```text
UDP Game Traffic
       ↓
      NLB
       ↓
Game Servers
```

### Shortcut

> **ALB = HTTP/HTTPS**
> **NLB = TCP/UDP + static IP**

---

# 9. Route 53 Routing Policies

You saw several of these.

### Weighted

> **Percentage**

```text
90% → Region A
10% → Region B
```

### Geolocation

> **Where is the user?**

```text
Japan → Tokyo
Sweden → Ireland
```

### Latency

> **Which endpoint provides the lowest latency?**

### Failover

> **Primary → Backup**

### Memory trick

**Weighted = Weight/percentage**

**Geolocation = Geography**

**Latency = Fastest**

**Failover = Backup**

---

# 10. CloudFront

CloudFront is a CDN.

Think:

> **Put content closer to users.**

Instead of:

```text
Pakistan ──────────────► US Origin
```

you get:

```text
Pakistan → CloudFront Edge → Origin
```

This reduces latency for cached content.

---

# 11. Lambda@Edge

You asked specifically about this.

Think:

> **Run Lambda logic at CloudFront edge locations.**

Useful when you need to customize requests/responses close to users.

For example:

```text
User
 ↓
CloudFront Edge
 ↓
Lambda@Edge
 ↓
Origin
```

Useful for things such as:

* authentication logic
* request modification
* response modification
* redirects
* personalization

---

# 12. CloudFront Origin Failover

If you have:

```text
CloudFront
    ↓
Primary Origin
```

and primary fails, CloudFront can use:

```text
Secondary Origin
```

This helps with availability and errors such as appropriate 5xx responses.

Think:

> **Origin group = primary + backup**

---

# 13. VPC Endpoints

If an EC2 instance in a private subnet needs to access:

* S3
* DynamoDB
* other supported AWS services

without going through the public Internet:

> **VPC Endpoint**

Example:

```text
Private EC2
    │
    ▼
VPC Endpoint
    │
    ├── S3
    └── DynamoDB
```

No NAT Gateway is required for this access pattern.

Important distinction:

> **Encryption ≠ network routing**

KMS/SSE protects data.

VPC endpoints control private connectivity.

---

# 14. Bastion Host

A bastion host is a controlled entry point into private instances.

Typical architecture:

```text
Corporate IP
     ↓
Bastion Host
(public subnet)
     ↓
Private EC2
Private EC2
Private EC2
```

For Windows:

> **RDP**

For Linux:

> **SSH**

The bastion should be accessible **only from trusted corporate IPs**, not from everywhere.

---

# 15. Transit Gateway

This is one of the most important networking concepts.

Imagine hundreds of VPCs.

Without Transit Gateway:

```text
VPC ↔ VPC
VPC ↔ VPC
VPC ↔ VPC
...
```

Very difficult to manage.

With Transit Gateway:

```text
        VPC
         |
VPC ─ Transit Gateway ─ VPC
         |
       VPN
         |
    On-premises
```

Think:

> **Transit Gateway = central network hub**

It supports connecting:

* VPCs
* VPNs
* Direct Connect gateways
* Transit Gateway peering

And Transit Gateway can be **peered across AWS Regions**.

---

# 16. AWS Organizations

When the requirement is:

> Multiple divisions/accounts + centralized governance + consolidated billing

Think:

**AWS Organizations**

```text
             Organization
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Account A Account B Account C
      Finance     Dev       HR
```

Each account maintains resource isolation/autonomy while the organization provides centralized management and billing.

Cross-account IAM roles can allow corporate administrators to access member accounts.

---

# 17. CloudFormation vs Terraform

You mentioned an important point:

> **You already use Terraform.**

That's completely fine.

For AWS SAA, understand the conceptual difference:

**CloudFormation**

> AWS-native Infrastructure as Code.

**Terraform**

> HashiCorp's multi-cloud Infrastructure as Code tool.

Both allow you to define infrastructure as code.

```text
Terraform / CloudFormation
          ↓
     Infrastructure
          ↓
 EC2 / VPC / S3 / RDS...
```

So don't worry that you haven't used CloudFormation extensively.

For the exam, know what CloudFormation **does**, not necessarily how to write YAML.

---

# 18. Serverless Containers

You asked about ECS, EKS and Fargate.

### ECS

AWS container orchestration service.

Think:

> **AWS-native container orchestration**

### EKS

Managed Kubernetes.

Think:

> **AWS-managed Kubernetes**

### Fargate

Serverless compute engine for containers.

Works with:

* ECS
* EKS

So:

```text
ECS + Fargate
       ↓
AWS-native containers
```

and:

```text
EKS + Fargate
       ↓
Kubernetes containers
```

### The important distinction

> **ECS ≠ Docker**

But your shortcut:

> "ECS is AWS's Docker"

is useful **as a beginner mental model**, as long as you remember it's technically an **AWS container orchestration service**, not Docker itself.

---

# 19. Lambda vs Fargate

Very important.

### Lambda

Think:

> **Function/serverless application execution**

Good for:

* event-driven workloads
* APIs
* short-running tasks
* serverless applications

### Fargate

Think:

> **Serverless containers**

Good when:

* you already have containers
* application needs more control
* long-running containers
* microservices

---

# 20. HPA / Cluster Autoscaler / Karpenter

These are Kubernetes concepts.

### HPA

**Horizontal Pod Autoscaler**

Changes:

> **number of pods**

```text
CPU high
 ↓
2 pods → 5 pods
```

### Cluster Autoscaler

Changes:

> **number of Kubernetes nodes**

```text
Pods can't be scheduled
       ↓
Cluster Autoscaler
       ↓
Add nodes
```

### Karpenter

Also provisions/terminates Kubernetes nodes based on pod requirements, with more flexible and rapid instance selection than traditional Cluster Autoscaler.

Think:

```text
HPA
 ↓
More Pods
 ↓
Not enough capacity
 ↓
Karpenter / Cluster Autoscaler
 ↓
More Nodes
```

For SAA, you mainly need to recognize the concepts rather than deeply implement them.

---

# 🏆 Your High-Performing Architecture Mental Map

If you remember nothing else, remember this:

```text
                 HIGH PERFORMANCE
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
    COMPUTE          STORAGE          NETWORK
       │               │                │
   EC2/ECS/EKS      EBS/EFS/FSx     ALB/NLB/CloudFront
   Lambda/Fargate      S3           Route 53/TGW
       │               │                │
       └───────────────┼────────────────┘
                       ↓
                   DATABASE
                       │
             ┌─────────┴─────────┐
             ↓                   ↓
         DynamoDB           RDS/Aurora
```

And your **high-value exam triggers**:

> **Memory metric → CloudWatch Agent**

> **DynamoDB change → DynamoDB Streams**

> **Streaming → Kinesis**

> **Streaming → S3/OpenSearch/Splunk → Firehose**

> **Windows + SMB + AD → FSx Windows**

> **HPC/ML parallel filesystem → FSx Lustre**

> **UDP → NLB**

> **HTTP path routing → ALB**

> **Static IP for firewall whitelist → NLB**

> **Percentage traffic → Route 53 Weighted**

> **Country → Route 53 Geolocation**

> **Lowest latency → Route 53 Latency**

> **Primary/backup → Route 53 Failover**

> **Private EC2 → S3/DynamoDB → VPC Endpoint**

> **Hundreds of VPCs → Transit Gateway**

> **Multiple company accounts + billing/governance → Organizations**

> **Kubernetes + serverless containers → EKS + Fargate**

> **Docker/container + serverless → Lambda container image or Fargate depending on workload**

> **Temporary Lambda files → `/tmp`**

---

# ✅ High-Performing Architecture: DONE

You've covered a pretty broad set of concepts through the questions rather than just memorizing definitions, which is exactly how I'd recommend preparing for SAA.

Now we can move to:

# 🔐 Secure Architectures

For the next section, I suggest we use the same approach:

**Question → identify clues → understand why correct → eliminate distractors → build an exam shortcut → maintain a running security summary.**

The major areas we'll likely encounter are:

* IAM
* IAM policies
* Roles vs users
* KMS
* S3 security
* encryption
* CloudTrail
* AWS Config
* GuardDuty
* Security Hub
* WAF
* Shield
* Secrets Manager
* Systems Manager
* VPC security groups/NACLs
* VPC endpoints
* least privilege
* cross-account access
* Organizations/SCPs
* data protection
* monitoring and auditing

So yes — **we can officially start Secure Architectures with your next question.**

15-August-2026
