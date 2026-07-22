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
