

1. iam
2. inspector vulnerable
3. aws security hub contral view
4. aws artifact


Yes. This question is basically testing **one new AWS service: AWS Artifact**. You don't need to think about application security here.

### First, understand the situation

Imagine your company is using AWS.

An **audit team** comes and says:

> "We need proof that AWS meets security and compliance standards such as SOC, PCI, etc. Give us the official compliance documents."

The question is asking:

**Where do we get AWS's official security/compliance reports?**

Answer: **AWS Artifact**.

### Add this to your AWS mental map

Think of these services like this:

| Service          | Simple meaning                                          |
| ---------------- | ------------------------------------------------------- |
| **IAM**          | Who can access AWS resources?                           |
| **Inspector**    | Are my workloads vulnerable?                            |
| **Security Hub** | Give me a central view of my security findings/posture  |
| **Artifact**     | Give me AWS's **official compliance reports/documents** |

So remember:

> **Artifact = AWS paperwork / compliance documents**

Examples of documents available through Artifact include **SOC reports, PCI reports, certifications, and certain AWS agreements**.

---

### Why the other answers are wrong

**Security Hub** ❌
Security Hub is more like:

> "Show me the security problems/findings across my AWS environment."

It is **not the place to download AWS compliance reports**.

**Inspector** ❌
Inspector asks:

> "Are my EC2 instances, containers, etc. vulnerable?"

It scans workloads for vulnerabilities. It doesn't provide AWS's compliance paperwork.

**IAM** ❌
IAM asks:

> "Who is allowed to do what?"

It has nothing to do with obtaining compliance reports.

### The exam shortcut

When you see:

* **AWS compliance reports**
* **SOC**
* **PCI**
* **certifications**
* **audit documents**
* **AWS compliance documentation**
* **agreements such as BAA/NDA**

👉 Think **AWS Artifact** immediately.

One important distinction:

**Artifact is about AWS's compliance documentation**, not about checking whether *your EC2/S3/application* is secure.

So your mental picture can be:

**Artifact → "Show me AWS's compliance paperwork."**
**Security Hub → "Show me my security posture/findings."**
**Inspector → "Find vulnerabilities in my workloads."**
**IAM → "Control access."**














23-August-2026

---
---
---



Perfect. Before moving to Lambda, here's a concise **SAA exam-focused IAM summary**.

---

# IAM Summary (SAA Cheat Sheet)

## 1. IAM Components

| Component  | Purpose                               |
| ---------- | ------------------------------------- |
| IAM User   | Permanent identity                    |
| IAM Group  | Manage permissions for multiple users |
| IAM Role   | Temporary permissions                 |
| IAM Policy | JSON document defining permissions    |

---

# 2. IAM User

* Permanent credentials
* Can have:

  * Console password
  * Access Keys
* Used by people or applications.

**Programmatic access**

→ Access Key + Secret Access Key

---

# 3. IAM Group

Use when multiple users need identical permissions.

Example:

100 Developers

↓

Developers Group

↓

Attach one S3 policy

Instead of attaching 100 policies.

---

# 4. IAM Role

Used for **temporary permissions**.

Common uses:

* EC2 → S3
* Lambda → DynamoDB
* ECS → S3
* Cross-account access
* Federation

**Exam keyword**

Temporary access

↓

IAM Role

---

# 5. AWS STS

(Security Token Service)

Creates temporary credentials.

Returns:

* Access Key
* Secret Key
* Session Token

Used with:

* AssumeRole
* Federation
* Cross-account access

---

# 6. Federation

Use existing corporate login.

Examples:

* Active Directory
* LDAP

No need to create IAM users.

---

## Active Directory + SAML

AD

↓

ADFS

↓

SAML

↓

STS

↓

IAM Role

---

## LDAP (No SAML)

LDAP

↓

Custom Identity Broker

↓

STS

↓

Temporary Credentials

---

# 7. IAM Identity Center

Old name:

AWS SSO

Purpose:

Central login for

* Multiple AWS accounts
* AWS Organizations
* Business applications

---

# 8. IAM Policies

Can attach to:

* User
* Group
* Role

Best practice:

Prefer Groups and Roles over attaching directly to users.

---

# 9. S3 Cross-Account Access

Need another AWS account to access your bucket?

Use:

* Bucket Policy ✅
* Cross-account IAM Role ✅

Avoid managing IAM users individually.

---

# 10. Security Services

| Service       | Purpose                          |
| ------------- | -------------------------------- |
| CloudTrail    | Records API calls                |
| GuardDuty     | Detects suspicious activity      |
| Inspector     | Finds vulnerabilities            |
| Macie         | Finds sensitive data (PII) in S3 |
| WAF           | Blocks SQL Injection & XSS       |
| Shield        | DDoS protection                  |
| Detective     | Investigates security findings   |
| Audit Manager | Compliance evidence              |
| Storage Lens  | S3 storage analytics             |

---

# High-Yield Exam Keywords

| Keyword                | Answer                       |
| ---------------------- | ---------------------------- |
| Temporary credentials  | STS                          |
| Cross-account access   | IAM Role + STS               |
| EC2 → S3               | IAM Role                     |
| Lambda → DynamoDB      | IAM Role                     |
| Many users             | IAM Group                    |
| Active Directory       | SAML Federation              |
| LDAP without SAML      | Custom Identity Broker + STS |
| API access             | Access Keys                  |
| Suspicious S3 activity | GuardDuty                    |
| Sensitive data in S3   | Macie                        |
| SQL Injection / XSS    | WAF                          |
| API audit              | CloudTrail                   |
| Vulnerability scanning | Inspector                    |

---

# 30-Second Decision Tree

* **Many users?** → IAM Group
* **Temporary permissions?** → IAM Role
* **Cross-account?** → STS + AssumeRole
* **Existing corporate login?** → Federation
* **Need API access?** → Access Keys
* **Sensitive S3 data?** → Macie
* **Suspicious AWS activity?** → GuardDuty
* **API history?** → CloudTrail
* **Web attacks?** → WAF
* **Find vulnerabilities?** → Inspector

---

## IAM Memory Formula

* **User = Permanent**
* **Group = Many Users**
* **Role = Temporary Access**
* **Policy = Permissions**
* **STS = Temporary Credentials**
* **SAML = Enterprise SSO**
* **Macie = Sensitive Data**
* **GuardDuty = Threat Detection**
* **CloudTrail = Audit Logs**
* **WAF = Web Protection**

---

You're ready to move on to **AWS Lambda**. It's another high-weight topic in the SAA exam and often appears with S3, API Gateway, EventBridge, DynamoDB, SQS, SNS, CloudWatch, and Step Functions. Understanding Lambda well will also make many serverless architecture questions much easier.


22-July-2026
