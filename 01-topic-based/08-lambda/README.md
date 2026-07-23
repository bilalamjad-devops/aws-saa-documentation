# AWS Lambda Summary (SAA-C03)

These are the **high-yield Lambda concepts** you need for the SAA exam.

---

# 1. What is AWS Lambda?

AWS Lambda is a **serverless compute service**.

* No servers to manage
* Auto scaling
* Event-driven
* Pay only when your code runs

---

# 2. Pricing

You pay for:

* Number of requests
* Execution duration
* Memory allocated

No charge when idle.

---

# 3. Execution Timeout

* Default: **3 seconds**
* Maximum: **900 seconds (15 minutes)**

### Exam Keyword

If Lambda:

* runs ~15 minutes
* terminates unexpectedly

➡️ **Reached maximum execution timeout (15 min).**

---

# 4. Common Lambda Triggers

* Amazon S3
* API Gateway
* EventBridge
* CloudWatch Events
* DynamoDB Streams
* Kinesis Data Streams
* SQS
* SNS

---

# 5. Lambda + API Gateway

Most common serverless architecture.

```text
Client
   │
   ▼
API Gateway
   │
   ▼
Lambda
```

Use when:

* REST APIs
* HTTP APIs
* Backend APIs
* Cost-effective serverless applications

---

# 6. Lambda + S3

Typical event-driven processing.

```text
S3 Upload
     │
     ▼
 Lambda
     │
     ▼
Process file
```

Common use cases:

* Image resize
* CSV processing
* Thumbnail generation
* File conversion
* Log processing

---

# 7. Lambda + Kinesis

For **real-time stream processing**.

```text
Kinesis
    │
    ▼
Lambda Consumer
    │
    ▼
Process
```

Lambda automatically:

* Polls stream
* Reads batches
* Retries failures
* Manages checkpoints

---

# 8. Lambda@Edge

Runs Lambda at **CloudFront Edge Locations**.

Use for:

* Low latency
* Global users
* Modify requests
* Modify responses
* Personalization
* Authentication near users
* Device detection

---

# 9. Lambda@Edge + CloudFront

```text
User

↓

CloudFront

↓

Lambda@Edge

↓

Origin
```

---

# 10. Lambda@Edge Use Cases

* Add HTTP headers
* Redirect users
* Authentication
* User-Agent detection
* WebP/JPEG selection
* Country-based content

---

# 11. Lambda@Edge + Kinesis

For:

* Clickstream analytics
* Global user tracking
* Real-time processing

Architecture:

```text
Users

↓

CloudFront

↓

Lambda@Edge

↓

Kinesis

↓

S3
```

---

# 12. Lambda + VPC

When Lambda needs access to:

* Amazon EFS
* Amazon RDS
* EC2
* ElastiCache

it must be attached to a **VPC**.

AWS creates **Elastic Network Interfaces (ENIs)** automatically.

```text
Lambda

↓

ENI

↓

VPC

↓

RDS / EFS
```

---

# 13. ENI/IP Exhaustion

If Lambda scales heavily inside a VPC, it needs:

* More ENIs
* More subnet IP addresses

If unavailable:

➡️ **EC2ThrottledException**

Common clue:

* Small subnet (/27, /28)
* High concurrency
* Lambda attached to VPC

---

# 14. Lambda vs Step Functions

### Lambda

* One task
* Event-driven
* Short processing

### Step Functions

* Multi-step workflows
* Branching
* Parallel execution
* Human approval
* Orchestration

---

# 15. Lambda vs Data Firehose

### Lambda

Custom processing

### Data Firehose

Managed delivery

```text
Streaming Data

↓

Firehose

↓

S3
Redshift
OpenSearch
```

---

# 16. Lambda vs ECS

### Lambda

* Event-driven
* REST APIs
* Auto scaling
* Cheapest
* No servers

### ECS/Fargate

* Docker containers
* Long-running services
* Microservices

---

# 17. Common Errors

| Error                    | Cause                                    |
| ------------------------ | ---------------------------------------- |
| Timeout                  | Exceeded configured timeout (max 15 min) |
| EC2ThrottledException    | ENI quota or subnet IP exhaustion        |
| EC2AccessDeniedException | Missing IAM permissions                  |
| Throttling               | Concurrent execution limit reached       |

---

# Exam Keywords → Answers

| Keywords                  | Answer                  |
| ------------------------- | ----------------------- |
| Serverless compute        | ✅ Lambda                |
| REST API                  | ✅ API Gateway + Lambda  |
| Event-driven              | ✅ Lambda                |
| S3 upload processing      | ✅ Lambda                |
| Image resize              | ✅ Lambda                |
| Real-time stream          | ✅ Kinesis + Lambda      |
| Global low latency        | ✅ Lambda@Edge           |
| Modify HTTP headers       | ✅ Lambda@Edge           |
| Authentication near users | ✅ Lambda@Edge           |
| WebP/JPEG selection       | ✅ Lambda@Edge           |
| Clickstream analytics     | ✅ Lambda@Edge + Kinesis |
| Lambda + RDS/EFS          | ✅ VPC                   |
| EC2ThrottledException     | ✅ ENI/IP exhaustion     |
| Multi-step workflow       | ✅ Step Functions        |
| Deliver stream to S3      | ✅ Data Firehose         |
| Containers                | ✅ ECS/Fargate           |

---

# Memory Tricks

* **REST API** → API Gateway + Lambda
* **S3 event** → Lambda
* **Streaming** → Kinesis + Lambda
* **Global edge processing** → Lambda@Edge
* **VPC resources (RDS/EFS)** → Lambda inside VPC
* **ENI/IP exhaustion** → EC2ThrottledException
* **15-minute execution** → Lambda timeout
* **Complex workflow** → Step Functions

---

## ✅ You're ready to start **Amazon RDS**.

For RDS, the exam heavily focuses on these topics:

1. **Read Replicas vs Multi-AZ**
2. **Backups & Snapshots**
3. **Aurora (very important)**
4. **RDS Proxy**
5. **Encryption**
6. **Storage Autoscaling**
7. **IAM Database Authentication**
8. **Performance Insights & Enhanced Monitoring**
9. **Failover behavior**
10. **Which database service to choose (RDS vs Aurora vs DynamoDB vs Redshift)**

These topics account for the vast majority of RDS questions you'll encounter on the SAA exam.


23-July-2026
