Perfect timing. Before moving to EBS, let's compress DynamoDB into an exam-focused cheat sheet.

# DynamoDB Summary for SAA

## 1. What is DynamoDB?

* Fully managed NoSQL database
* Serverless
* Extremely scalable
* Stores data as:

  * Tables
  * Items (rows)
  * Attributes (columns)

Think:

```text
RDS = SQL
DynamoDB = NoSQL
```

---

# 2. Primary Keys

### Partition Key

```text
UserID
```

AWS uses this to determine where data is stored.

Example:

```text
UserID = 101
UserID = 102
UserID = 103
```

---

### Composite Key

```text
UserID + OrderID
```

Contains:

* Partition Key
* Sort Key

Example:

```text
UserID = 101
OrderID = 1

UserID = 101
OrderID = 2

UserID = 101
OrderID = 3
```

---

# 3. High Cardinality (Very Important)

Good:

```text
UserID
Email
TransactionID
```

Bad:

```text
Country
Gender
Status
```

Reason:

```text
High Cardinality
=
More unique values
=
Better distribution
=
Avoid Hot Partitions
```

Exam clue:

> Uneven WCU/RCU consumption

Answer:

✅ High-cardinality partition key

---

# 4. RCU and WCU

### RCU

Read Capacity Units

### WCU

Write Capacity Units

Example:

```text
Traffic increases
↓
RCU exhausted
↓
Throttling
```

Solution:

✅ Increase capacity
or
✅ Enable Auto Scaling

---

# 5. DynamoDB Auto Scaling

Just like EC2 Auto Scaling.

You configure:

```text
Minimum Capacity
Maximum Capacity
Target Utilization
```

Example:

```text
Min RCU = 100
Max RCU = 1000
Target = 70%
```

AWS automatically adjusts capacity.

Exam clue:

> Table throttling during peak traffic

Answer:

✅ DynamoDB Auto Scaling

---

# 6. DAX (DynamoDB Accelerator)

In-memory cache for DynamoDB.

Without DAX:

```text
App
 ↓
DynamoDB
```

With DAX:

```text
App
 ↓
DAX Cache
 ↓
DynamoDB
```

Performance:

```text
Milliseconds
↓
Microseconds
```

Exam clue:

> Millions of reads
> Improve read performance

Answer:

✅ DAX

---

# 7. DynamoDB Streams

Captures:

```text
INSERT
UPDATE
DELETE
```

events.

Example:

```text
New record added
↓
Stream event generated
↓
Lambda triggered
```

Exam clue:

> Detect changes
> Trigger Lambda
> Send notification

Answer:

✅ DynamoDB Streams + Lambda

---

# 8. DynamoDB Streams vs DAX

Students confuse these.

### DAX

```text
Performance
Caching
Fast Reads
```

### Streams

```text
Change Detection
Event Processing
Triggers
```

---

# 9. Global Secondary Index (GSI)

Extra way to query data.

Example:

Table key:

```text
UserID
```

Need queries by:

```text
Email
```

Create:

```text
GSI on Email
```

Exam clue:

> Need alternative query pattern

Answer:

✅ GSI

---

# 10. Global Tables

Multi-region DynamoDB.

```text
US East
    ⇄
EU West
```

Automatic replication.

Exam clue:

> Multi-region
> DR
> Low latency worldwide

Answer:

✅ Global Tables

---

# 11. Backup Features

### PITR

Point-In-Time Recovery

Restore table to a previous time.

Example:

```text
2:35 PM
```

Can restore within same account/region.

---

### AWS Backup

Used when:

```text
Cross-account backup
Cross-region backup
Centralized backup management
```

Exam clue:

> Copy DynamoDB backup to another account

Answer:

✅ AWS Backup

NOT PITR

---

# 12. VPC Endpoint

DynamoDB supports:

✅ Gateway Endpoint

```text
VPC
 ↓
Gateway Endpoint
 ↓
DynamoDB
```

Traffic stays on AWS network.

Exam clue:

> Access DynamoDB without public internet

Answer:

✅ DynamoDB Gateway Endpoint

---

# 13. Common Exam Associations

| Requirement                   | Service              |
| ----------------------------- | -------------------- |
| Faster reads                  | DAX                  |
| Auto adjust RCU/WCU           | Auto Scaling         |
| Trigger action on data change | Streams              |
| Lambda after table update     | Streams + Lambda     |
| Multi-region replication      | Global Tables        |
| Alternate query pattern       | GSI                  |
| Restore previous version      | PITR                 |
| Cross-account backup          | AWS Backup           |
| Private access from VPC       | Gateway Endpoint     |
| Avoid hot partitions          | High Cardinality Key |

---

# Final Memory Sentence

If I had to remember DynamoDB in one line:

> **DAX speeds it up, Auto Scaling handles traffic, Streams detect changes, Global Tables replicate data, GSI improves queries, and AWS Backup handles cross-account recovery.**

You're ready to move to **EBS** now. EBS is actually much smaller than DynamoDB for SAA and revolves around about 10 recurring concepts:

* Volume types (gp3, io2, st1, sc1)
* Snapshots
* Encryption
* Multi-Attach
* Resize volumes
* EBS vs Instance Store
* AMI relationship
* Snapshot copy across regions
* Incremental snapshots
* Root volume behavior

Those are the ones that appear again and again in exam questions.


20-July-2026
