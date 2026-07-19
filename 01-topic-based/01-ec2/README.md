Absolutely. Before starting Auto Scaling Groups (ASG), let's create a **one-page EC2 cheat sheet**. If you know everything below, you're in excellent shape for the SAA exam.

---

# EC2 SAA Cheat Sheet

## 1. EC2 Purchasing Options

| Type                        | Best For                                 | Cost       | Can AWS Interrupt?      |
| --------------------------- | ---------------------------------------- | ---------- | ----------------------- |
| **On-Demand**               | Short-term, unpredictable workloads      | 💰💰💰💰   | ❌ No                    |
| **Spot**                    | Batch jobs, video transcoding, CI/CD, ML | 💰         | ✅ Yes (2-minute notice) |
| **Reserved Instances**      | Long-running workloads (1–3 years)       | 💰💰       | ❌ No                    |
| **Savings Plans**           | Flexible long-term compute savings       | 💰💰       | ❌ No                    |
| **Dedicated Host/Instance** | Licensing & compliance                   | 💰💰💰💰💰 | ❌ No                    |

### Remember

* **Interruptible workload** → Spot
* **24/7 predictable workload** → Reserved Instance / Savings Plan
* **Temporary workload** → On-Demand
* **Licensing requirement** → Dedicated Host

---

# 2. IAM for EC2

❌ Never store Access Key and Secret Key on EC2.

✅ Attach an **IAM Role**.

```text
EC2
   │
IAM Role
   │
S3 / DynamoDB / SNS
```

Exam keyword:

> EC2 needs access to AWS services → **IAM Role**

---

# 3. CloudWatch Metrics

## Default Metrics

✅ CPU

✅ Network In/Out

✅ Disk Read/Write IOPS

✅ Status Checks

---

## Need CloudWatch Agent

❌ Memory

❌ Disk Space Used

❌ Swap

❌ Logs

❌ Processes

Golden Rule:

> **Operating System metrics require the CloudWatch Agent.**

---

# 4. Capacity Reservation

Difference between **capacity** and **discount**.

### Capacity Reservation

Guarantees hardware.

Example:

Need EC2 every night from 10 PM–3 AM.

Use:

> On-Demand Capacity Reservation

---

### Reserved Instance

Gives billing discount.

Not necessarily capacity (Regional RI).

---

Remember:

| Service              | Capacity | Discount |
| -------------------- | -------- | -------- |
| Capacity Reservation | ✅        | ❌        |
| Regional RI          | ❌        | ✅        |
| Zonal RI             | ✅        | ✅        |
| Savings Plan         | ❌        | ✅        |

---

# 5. High Availability vs Fault Tolerance

### High Availability

```text
AZ A     AZ B

EC2      EC2
```

Application survives an AZ failure.

---

### Fault Tolerance

```text
AZ A

EC2
EC2

AZ B

EC2
EC2
```

Even after an AZ fails, you still have the required capacity.

Exam keyword:

Mission Critical

↓

Fault Tolerant

↓

Extra redundant resources

---

# 6. EC2 Service Quotas

AWS limits EC2 launches.

Today it's primarily based on:

> **Regional vCPU quota**

If launches fail:

Solution:

Increase Service Quota.

Not:

❌ Change AZ

❌ Retry forever

---

# 7. IPv4 vs IPv6 Internet Access

### IPv4

Private EC2

↓

NAT Gateway

↓

Internet Gateway

↓

Internet

---

### IPv6

Private EC2

↓

Egress-Only Internet Gateway

↓

Internet

Remember:

IPv4 → NAT

IPv6 → Egress-Only IGW

---

# 8. Security Services

| Service              | Purpose                         |
| -------------------- | ------------------------------- |
| IAM Role             | Authentication                  |
| GuardDuty            | Threat Detection                |
| AWS Network Firewall | Traffic Inspection & Filtering  |
| Firewall Manager     | Centralized Firewall Management |
| Traffic Mirroring    | Copy Packets                    |
| PrivateLink          | Private access to AWS services  |

---

# 9. Default EC2 Architecture

```text
Internet

↓

ALB

↓

Auto Scaling Group

↓

EC2

↓

RDS
```

This architecture appears throughout the SAA exam.

---

# 10. EC2 Exam Keywords

| If you see...         | Think...                             |
| --------------------- | ------------------------------------ |
| Memory                | CloudWatch Agent                     |
| EC2 → S3              | IAM Role                             |
| Can be interrupted    | Spot Instances                       |
| Dedicated hardware    | Dedicated Host                       |
| Reserve hardware      | Capacity Reservation                 |
| Long-running workload | Reserved Instance / Savings Plan     |
| IPv6 outbound only    | Egress-Only Internet Gateway         |
| Traffic inspection    | AWS Network Firewall                 |
| Launch limit reached  | Service Quotas (regional vCPU limit) |
| Mission-critical      | Multi-AZ + Fault Tolerance           |

---

# 11. Most Important EC2 Decision Tree (Memorize This)

```text
Need EC2?

│
├── Temporary?
│      │
│      ├── Can be interrupted?
│      │         │
│      │         ├── YES → Spot
│      │         └── NO → On-Demand
│
└── Long-term?
       │
       ├── Want discount?
       │         │
       │         ├── YES → Savings Plan / Reserved Instance
       │
       └── Need dedicated hardware?
                 │
                 └── Dedicated Host
```

---

# ⭐ Bilal's Golden Rules (Remember These)

1. **EC2 → AWS Service = IAM Role**
2. **Memory/Logs/Disk Usage = CloudWatch Agent**
3. **Interruptible Workload = Spot Instances**
4. **Mission Critical = Multi-AZ**
5. **Fault Tolerance = Extra Redundancy**
6. **IPv4 Private Internet = NAT Gateway**
7. **IPv6 Private Internet = Egress-Only Internet Gateway**
8. **Traffic Inspection = AWS Network Firewall**
9. **Guaranteed EC2 Capacity = Capacity Reservation**
10. **Long-running Cost Savings = Savings Plans / Reserved Instances**

---

## Ready for Auto Scaling Groups (ASG)

ASG is one of the biggest topics in the SAA exam. We'll cover:

1. **Launch Templates**
2. **Desired vs Minimum vs Maximum Capacity**
3. **Scaling Policies**
4. **Target Tracking**
5. **Dynamic Scaling**
6. **Scheduled Scaling**
7. **Health Checks (EC2 & ELB)**
8. **Cooldowns and Warmup**
9. **Lifecycle Hooks**
10. **Instance Refresh**
11. **Termination Policies**
12. **ASG with ALB**
13. **ASG Exam Traps**

Once you master ASGs, you'll have one of the most heavily tested EC2 topics covered.

18-July-2026

19-July-2026
