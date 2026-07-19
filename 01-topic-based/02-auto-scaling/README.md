You're welcome, Bilal. Before moving to DynamoDB, let's create a **one-page ASG revision sheet**. This is exactly what I'd want you to review the night before the SAA exam.

---

# Amazon EC2 Auto Scaling (ASG) – SAA Revision Sheet

## 1. What is Auto Scaling?

Auto Scaling automatically:

* Launches EC2 instances when demand increases.
* Terminates EC2 instances when demand decreases.
* Maintains application availability.
* Reduces infrastructure costs.

Think:

```text
Traffic ↑
    ↓
ASG launches EC2

Traffic ↓
    ↓
ASG terminates EC2
```

---

# 2. Capacity Settings

Every ASG has three important values.

### Minimum Capacity

Never go below this number.

Example:

```text
Min = 2
```

Even if there is zero traffic,

2 EC2 instances stay running.

---

### Desired Capacity

Number of instances ASG tries to maintain.

Example:

```text
Desired = 4
```

ASG continuously works to keep 4 running instances.

---

### Maximum Capacity

Never exceed this number.

Example:

```text
Max = 10
```

Even during huge traffic,

ASG won't launch more than 10.

---

# 3. High Availability

Always deploy ASG across **multiple Availability Zones**.

Good

```text
AZ-A     AZ-B

EC2      EC2
EC2      EC2
```

Bad

```text
AZ-A

EC2
EC2
EC2
EC2
```

If AZ-A fails,

Everything is gone.

---

# 4. Scaling Policies

## Scheduled Scaling

Use when traffic is predictable.

Example:

```text
Every day

8 AM

↓

Launch more EC2
```

Keyword:

**Known schedule**

---

## Dynamic Scaling

Scale using CloudWatch metrics.

Example:

```text
CPU > 70%

↓

Launch EC2
```

Keyword:

**Metric-based**

---

## Target Tracking Scaling

You define a target.

Example:

```text
Keep CPU

≈ 50%
```

AWS automatically decides how much to scale.

Memory trick:

Thermostat.

---

## Step Scaling

Different actions depending on alarm severity.

Example:

```text
CPU > 60%

↓

+1 EC2
```

```text
CPU > 75%

↓

+2 EC2
```

```text
CPU > 90%

↓

+4 EC2
```

Keyword:

**Set of scaling adjustments**

---

## Simple Scaling

Only one rule.

Example:

```text
CPU > 70%

↓

+1 EC2
```

Older and less flexible than Step Scaling.

---

# 5. Cooldown

Purpose:

Prevent repeated scaling actions too quickly.

Default:

```text
300 seconds
```

Memory:

**ASG waits before making another scaling decision.**

---

# 6. Instance Warm-up

Purpose:

Allow newly launched EC2 instances to become fully operational before using them in scaling decisions.

Example:

```text
Launch

↓

Boot

↓

Health Check

↓

Warm-up Complete

↓

Receive Traffic
```

Memory:

**New EC2 needs time to become ready.**

---

# 7. Launch Template

Blueprint used by ASG.

Contains:

* AMI
* Instance Type
* Security Group
* IAM Role
* Key Pair
* User Data
* Storage

Need a new AMI?

Create a **new Launch Template (or new Launch Template version)** and update the ASG.

---

# 8. Default Termination Policy

When scaling in,

AWS first tries to:

1. Keep Availability Zones balanced.
2. Terminate instances using the oldest Launch Template.
3. Use additional tie-breakers if needed.

Memory:

**Balance first. Old template second.**

---

# 9. Queue-Based Scaling

Very common architecture.

```text
Application

↓

SQS

↓

Auto Scaling

↓

EC2 Workers
```

If queue grows,

Launch more EC2.

If queue shrinks,

Terminate EC2.

---

# 10. Common AWS Services with ASG

## ALB

Distributes HTTP/HTTPS traffic.

---

## Launch Template

Creates EC2.

---

## CloudWatch

Monitors metrics.

Triggers scaling.

---

## SQS

Buffers work.

Can trigger scaling.

---

## Target Group

Receives traffic from ALB.

Contains EC2 instances.

---

# 11. Important Exam Traps

| Question Says                | Think                      |
| ---------------------------- | -------------------------- |
| Predictable traffic          | Scheduled Scaling          |
| Maintain CPU at 50%          | Target Tracking            |
| Multiple scaling adjustments | Step Scaling               |
| Single adjustment            | Simple Scaling             |
| Wait before scaling again    | Cooldown                   |
| New EC2 takes time to boot   | Instance Warm-up           |
| New AMI                      | Launch Template            |
| Remove old EC2               | Default Termination Policy |
| Message buffer               | SQS                        |
| Email/SMS notifications      | SNS                        |
| Workflow orchestration       | Step Functions             |

---

# 12. Real Production Architecture

```text
                 Internet
                     │
                     ▼
          Application Load Balancer
                     │
              Target Group
                     │
           Auto Scaling Group
                     │
            Launch Template
                     │
             EC2 Instances
                     │
              CloudWatch
                     ▲
                     │
               CPU / Queue
```

---

# 13. Bilal's Golden Rules

### Rule #13

Cooldown is for the **Auto Scaling Group**, not for EC2 instances.

### Rule #14

Scale-in → Balance Availability Zones first, then remove instances using the oldest Launch Template.

### Rule #15

Need a new AMI? Update the Launch Template used by the ASG (typically via a new template version).

### Rule #16

Maintain a metric → Target Tracking.

Multiple adjustment levels → Step Scaling.

### Rule #17

Boot delay → Instance Warm-up.

Repeated scaling → Cooldown.

### Rule #18

Distributed application + message buffer + scalable workers → **Amazon SQS + Auto Scaling Group**.

---

# Auto Scaling Complete! 🎉

You now have a solid foundation. Most ASG questions on the SAA exam revolve around:

* Scaling policies
* Launch Templates
* Multi-AZ deployments
* Warm-up vs. Cooldown
* SQS-driven worker scaling

---

# Next Topic: Amazon DynamoDB

DynamoDB is one of the most frequently tested services in SAA. We'll cover it in this order:

1. What DynamoDB is and when to use it.
2. Primary Keys (Partition Key vs. Composite Key).
3. Read/Write Capacity Modes (Provisioned vs. On-Demand).
4. Eventually Consistent vs. Strongly Consistent Reads.
5. Local Secondary Index (LSI) vs. Global Secondary Index (GSI).
6. DynamoDB Accelerator (DAX).
7. DynamoDB Streams.
8. Time to Live (TTL).
9. Global Tables.
10. Backup & Restore.
11. DynamoDB Auto Scaling.
12. Common SAA exam traps.

We'll learn it the same way—one Tutorials Dojo question at a time. I think DynamoDB will become much less intimidating once we build it from first principles.

19-July-2026
