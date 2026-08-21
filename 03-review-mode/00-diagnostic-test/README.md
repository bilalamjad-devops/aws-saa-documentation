
Absolutely. Before moving to **Review Mode Set 1**, let's lock down Questions **1–7**. You actually touched several important SAA patterns here.

# SAA Review Mode — Questions 1–7 Summary

## Q1 — EKS + URL Path Routing

**Scenario:** EKS hosts microservices and requests must be routed based on **URL paths**.

Example:

```text
example.com/orders  → Orders service
example.com/users   → Users service
example.com/payments → Payments service
```

### Correct

**ALB + AWS Load Balancer Controller**

Why?

* **ALB = Layer 7**
* ALB understands HTTP/HTTPS and can route based on:

  * URL path
  * Hostname
  * HTTP headers, etc.
* AWS Load Balancer Controller integrates Kubernetes Ingress with AWS ALB.

### Remember

| Load Balancer |                       Layer | Main routing                  |
| ------------- | --------------------------: | ----------------------------- |
| ALB           |                          L7 | Path, host, HTTP              |
| NLB           |                          L4 | IP, port, TCP/UDP             |
| GWLB          | L3-ish / network appliances | Firewalls/security appliances |

**Keyword:**

> URL path → **ALB**

---

# Q2 — Lambda Cold Starts: SnapStart vs Provisioned Concurrency

**Scenario:** Java Lambda application. Need to reduce **cold starts/outlier latency** while keeping costs down.

### Correct

**Lambda SnapStart**

SnapStart works especially well for supported Java runtimes.

Think:

```text
Normal Lambda

Request
   ↓
Create environment
   ↓
Initialize Java
   ↓
Run function
```

The initialization can cause a cold start.

With SnapStart:

```text
Function published
      ↓
Initialize Java environment
      ↓
Take snapshot
      ↓
Cache snapshot
      ↓
Request → restore initialized environment → run
```

So it avoids repeatedly doing the expensive initialization work.

### Why not Provisioned Concurrency?

Provisioned Concurrency also reduces cold starts, but:

> **You pay for keeping execution environments provisioned.**

So if the question emphasizes:

**Java + cold starts + cost-effective**

→ **SnapStart**

### Important distinction

**Provisioned Concurrency**

> "Keep Lambda environments warm."

**SnapStart**

> "Take a snapshot of an initialized environment and restore it quickly."

---

# Q3 — AWS Organizations + OU Changes

**Scenario:** Need to monitor changes to the **Organizational Unit hierarchy** and notify stakeholders with minimal administration.

### Correct

**AWS Control Tower + account drift notifications**

Control Tower is designed to help govern a multi-account AWS environment.

Think:

```text
AWS Organizations
       ↓
     OUs
       ↓
Accounts
       ↓
Control Tower governance
       ↓
Drift notifications
```

### Important distinction

**AWS Config**

→ monitors **resource configuration/compliance**

Example:

> Is this S3 bucket public?

It isn't primarily the tool for:

> "Someone changed my OU hierarchy."

**CloudTrail**

→ records API activity.

It can tell you that someone called an Organizations API, but you'd need additional logic to turn those events into the specific monitoring/alerting solution.

### Remember

> **Multi-account governance → Control Tower**

> **Resource configuration → Config**

> **API activity/audit → CloudTrail**

---

# Q4 — Lambda Throttling + SQS

This is an **important architecture question**.

### Scenario

You have:

```text
API Gateway
     ↓
 Lambda
     ↓
 Aurora
```

Traffic suddenly increases.

Lambda starts getting too many concurrent executions → **throttling**.

But the application only needs to say:

> "I received your request."

It does **not** need to process the request immediately.

### Correct architecture

```text
API Gateway
     ↓
Lambda #1
     ↓
   SQS
     ↓
Lambda #2
     ↓
 Aurora
```

### Why SQS?

SQS acts as a **buffer**.

Imagine 10,000 requests arrive:

```text
Without SQS:

10,000 requests
       ↓
10,000 Lambda executions
       ↓
Throttling 😵
```

With SQS:

```text
10,000 requests
       ↓
      SQS
       ↓
Lambda processes at manageable rate
       ↓
    Aurora
```

The queue absorbs traffic spikes.

### SQS vs SNS

This is extremely important:

**SQS = queue / buffer / decoupling**

**SNS = pub/sub / fanout**

Example SNS:

```text
SNS
 ├── Lambda
 ├── SQS
 └── Email
```

One message → multiple subscribers.

Example SQS:

```text
Producer
   ↓
 SQS
   ↓
Consumer
```

Messages wait until consumers process them.

### Shortcut

> **Need buffering → SQS**

> **Need fanout → SNS**

---

# Q5 — Aurora Serverless v2

**Scenario:**

Existing database:

```text
On-premises MySQL-compatible DB
```

Memory usage:

```text
2 GiB → 16 GiB
```

Traffic is unpredictable.

Need:

> Managed + automatically scale capacity + MySQL compatibility.

### Correct

**Aurora Serverless v2**

The key is **serverless database capacity scaling**.

Aurora Serverless v2 uses **ACUs (Aurora Capacity Units)**.

Approximately:

> **1 ACU ≈ 2 GiB memory**

Therefore:

```text
2 GiB  → 1 ACU
16 GiB → 8 ACUs
```

So:

```text
Minimum = 1 ACU
Maximum = 8 ACUs
```

### Important distinction

**RDS MySQL**

```text
Choose instance
     ↓
Fixed compute/memory
```

It is managed, but the instance capacity itself doesn't automatically scale like Aurora Serverless v2.

**Aurora provisioned**

Also uses provisioned DB instances.

**Aurora Serverless v2**

```text
Low traffic → scale down
High traffic → scale up
```

### What about DynamoDB?

Yes, DynamoDB also has automatic scaling/serverless characteristics, **but it is NoSQL**.

If the question says:

> Existing MySQL-compatible application/database

Aurora Serverless v2 is usually the natural answer.

### Your earlier question

You asked:

> "Aurora v2 and DynamoDB both can scale right?"

**Yes**, but don't treat them as interchangeable.

```text
Aurora Serverless v2
→ Relational
→ MySQL/PostgreSQL compatible

DynamoDB
→ NoSQL
→ Requires different data model/application approach
```

---

# Q6 — EC2/RDS Only During Working Hours

Scenario:

```text
Monday-Friday
Working hours only
```

Resources:

* EC2
* RDS

Need:

> Reduce cost + minimal operational overhead.

### Correct

**Instance Scheduler on AWS**

It automatically starts/stops EC2 and RDS according to a schedule.

Example:

```text
8 AM  → START

5 PM  → STOP
```

Instead of:

```text
24 hours × 7 days
```

you pay for resources primarily when they're actually needed.

### Why not Reserved Instances?

Reserved Instances are useful when you have **steady usage**.

Example:

```text
24/7
365 days
```

But this workload is:

```text
Only weekdays
Only working hours
```

So paying for a long-term reservation isn't the best fit.

### Why not Savings Plans?

Same general idea:

> Best when you have consistent compute usage.

Also, Compute Savings Plans don't cover RDS DB instances.

### Important correction from your earlier question

**Instance Scheduler on AWS is an AWS-provided solution/template implemented through CloudFormation.**

You don't need to think:

> "CloudFormation itself is the scheduler."

Instead:

```text
AWS-provided Instance Scheduler solution
             ↓
     deployed using CloudFormation
             ↓
 manages EC2/RDS schedules
```

You can also build your **own equivalent automation using Terraform, Lambda, EventBridge, etc.**

But for an exam question asking specifically for **Instance Scheduler on AWS**, the expected answer is the AWS-provided solution.

---

# Q7 — Unexpected AWS Spending

Scenario:

> "Unusual spending patterns"

### Correct

**AWS Cost Anomaly Detection**

This is one of those questions where the wording gives away the answer.

### Cost tools

| Tool                       | Think                          |
| -------------------------- | ------------------------------ |
| **Cost Explorer**          | Analyze costs                  |
| **AWS Budgets**            | Set spending thresholds        |
| **Cost Anomaly Detection** | Detect unusual spending        |
| **CloudWatch**             | Monitor resources/applications |

### Example

Normal:

```text
Monthly S3 cost ≈ $300
```

Suddenly:

```text
S3 cost → $1,200
```

Cost Anomaly Detection can identify the unusual spending pattern and alert you.

### Shortcut

> **Unexpected / anomalous spending → Cost Anomaly Detection**

---

# 🔥 The 7 Questions — One-Page Cheat Sheet

| Q     | Scenario                           | Answer                                 | Key concept                |
| ----- | ---------------------------------- | -------------------------------------- | -------------------------- |
| **1** | EKS URL path routing               | **ALB + AWS Load Balancer Controller** | L7/path routing            |
| **2** | Java Lambda cold starts + cost     | **Lambda SnapStart**                   | Reduce Java cold starts    |
| **3** | Organizations OU changes           | **Control Tower drift notifications**  | Multi-account governance   |
| **4** | Lambda throttling / traffic spikes | **SQS + Lambda**                       | Buffer & decouple          |
| **5** | MySQL + unpredictable DB load      | **Aurora Serverless v2**               | Relational + auto capacity |
| **6** | EC2/RDS only business hours        | **Instance Scheduler**                 | Start/stop resources       |
| **7** | Unexpected AWS spending            | **Cost Anomaly Detection**             | Detect abnormal costs      |

---

# 🧠 Most Important Patterns So Far

You should start recognizing these **keywords**, rather than memorizing the entire explanations.

### Architecture

**URL path routing**
→ ALB

**Lambda throttling + asynchronous processing**
→ SQS

**Fanout**
→ SNS

**Traffic spike + buffer**
→ SQS

---

### Compute

**Java Lambda cold start**
→ SnapStart

**Eliminate cold starts by keeping environments warm**
→ Provisioned Concurrency

---

### Database

**MySQL/PostgreSQL + unpredictable load + serverless scaling**
→ Aurora Serverless v2

**NoSQL + serverless + massive scale**
→ DynamoDB

---

### Multi-account

**AWS Organizations + governance**
→ Control Tower

**Resource configuration/compliance**
→ AWS Config

**API activity/audit**
→ CloudTrail

---

### Cost

**Unexpected spending**
→ Cost Anomaly Detection

**Set spending threshold**
→ AWS Budgets

**Analyze historical costs**
→ Cost Explorer

**Only need EC2/RDS during certain hours**
→ Instance Scheduler

---

## Your weak areas from Q1–7

I would particularly mark these for another pass:

**🟡 Lambda:** SnapStart vs Provisioned Concurrency
**🟡 SQS vs SNS**
**🟡 Aurora Serverless v2 vs DynamoDB vs RDS**
**🟡 Control Tower vs Config vs CloudTrail**
**🟡 AWS cost-management tools**

The good news is that these are **conceptual distinctions**, not huge topics. Once you recognize the keywords, many SAA questions become much easier.

Now we can move to **Review Mode Set 1 — Question 1/50** and use the same approach: **what is the question really asking → eliminate options → why the correct answer wins → exam shortcut.**

21-August-2026
