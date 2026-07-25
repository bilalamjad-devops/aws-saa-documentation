Great job. You've finished the **high-yield SQS topics**. Before moving to VPC, let's summarize everything in the simplest way possible.

---

# Amazon SQS (Simple Queue Service)

## What is SQS?

SQS is a **message queue** that sits between two applications.

Instead of applications talking directly, they communicate through the queue.

```text
Producer
    │
    ▼
+-----------+
|    SQS    |
+-----------+
    │
    ▼
Consumer
```

**Purpose:** Decouple applications.

---

# Why use SQS?

Without SQS

```text
Web App ─────► Worker
```

Worker crashes?

❌ Request is lost.

---

With SQS

```text
Web App
   │
   ▼
 SQS Queue
   │
   ▼
 Worker
```

Worker crashes?

✅ Message stays in queue.

Another worker can process it later.

---

# Long Polling vs Short Polling

## Short Polling (Default)

```text
Worker:
Any message?
No

Any message?
No

Any message?
No
```

* More API calls
* More empty responses
* Higher cost

Default:

```
ReceiveMessageWaitTimeSeconds = 0
```

---

## Long Polling

```text
Worker:
Any message?

(wait...)

(wait...)

Yes!
```

AWS waits until a message arrives.

Benefits:

* Lower cost
* Fewer API calls
* Fewer empty responses

Setting:

```
ReceiveMessageWaitTimeSeconds > 0
```

---

# Visibility Timeout

Suppose one message exists.

```text
Queue

Order #101
```

Worker A receives it.

AWS hides the message.

```text
Invisible
```

Worker B cannot receive it.

If Worker A finishes:

```
DeleteMessage()
```

Message disappears permanently.

If Worker A crashes:

Visibility timeout expires.

Message becomes visible again.

Another worker processes it.

**Default:** 30 seconds

**Maximum:** 12 hours

---

# Message Retention Period

Question:

> How long does SQS keep an unprocessed message?

Default:

```
4 days
```

Maximum:

```
14 days
```

Example:

```text
Day 1
Message arrives

Day 5
Deleted automatically
```

If your application runs once a week, messages may already be gone.

---

# Dead Letter Queue (DLQ)

Suppose processing keeps failing.

```text
Try 1 ❌

Try 2 ❌

Try 3 ❌
```

Instead of retrying forever

↓

Move message to

```text
Dead Letter Queue
```

Used for debugging failed messages.

---

# SNS + SQS Fanout

One message

↓

Many consumers

```text
          SNS
           │
    ┌──────┼──────┐
    ▼      ▼      ▼
 Queue1 Queue2 Queue3
```

Each queue receives a copy.

---

# SNS Filter Policy

Without filter

Every queue receives every message.

With filter

```text
SNS

│

├── Car Queue
├── Home Queue
└── Health Queue
```

Each queue only receives the messages it needs.

---

# Priority Processing

SQS **does not support message priority.**

Wrong:

```
High Priority
Low Priority
```

Correct:

```text
Premium Queue

Free Queue
```

Workers check:

1. Premium Queue
2. Free Queue

---

# Standard vs FIFO

## Standard Queue

* Unlimited throughput
* Possible duplicates
* Best-effort ordering
* Default

---

## FIFO Queue

* First In First Out
* Exactly-once processing (with deduplication features)
* Message ordering guaranteed

---

# SNS vs SQS

| SNS                  | SQS                     |
| -------------------- | ----------------------- |
| Push messages        | Consumers poll messages |
| Notification service | Queue service           |
| One → Many           | One queue               |
| Fanout               | Decoupling              |

Think:

> **SNS announces.**

> **SQS stores.**

---

# Complete SQS Cheat Sheet

| Requirement                   | AWS Service/Feature        |
| ----------------------------- | -------------------------- |
| Decouple applications         | SQS                        |
| Buffer requests               | SQS                        |
| One message → many systems    | SNS                        |
| Fanout                        | SNS → SQS                  |
| Filter messages               | SNS Filter Policy          |
| Reduce polling cost           | Long Polling               |
| Prevent duplicate processing  | Visibility Timeout         |
| Consumer crashes              | Visibility Timeout expires |
| Store failed messages         | Dead Letter Queue          |
| Premium users first           | Multiple SQS queues        |
| Ordered processing            | FIFO Queue                 |
| Duplicate removal             | FIFO Queue                 |
| Messages disappear after days | Message Retention Period   |

---

# Memory Rules for the Exam

When you read...

* **"Decouple"** → SQS
* **"Buffer requests"** → SQS
* **"Many subscribers"** → SNS
* **"Notify multiple systems"** → SNS
* **"Reduce empty responses"** → Long Polling
* **"Prevent another worker from processing the same message"** → Visibility Timeout
* **"Consumer crashed"** → Visibility Timeout
* **"Message missing after several days"** → Message Retention Period
* **"Failed repeatedly"** → Dead Letter Queue
* **"VIP/Premium users"** → Multiple SQS queues
* **"Message order matters"** → FIFO Queue

---

# Next Topic: Amazon VPC

VPC is one of the biggest sections of the SAA exam. We'll build it step by step:

1. **What is a VPC?** (Virtual network)
2. CIDR Blocks
3. Public vs Private Subnets
4. Internet Gateway (IGW)
5. Route Tables
6. NAT Gateway
7. Security Groups
8. Network ACLs
9. Bastion Host
10. VPC Endpoints
11. VPC Peering
12. Transit Gateway
13. VPN & Direct Connect

Once you understand those concepts, you'll be able to solve the majority of VPC questions in the SAA exam.

26-July-2026
