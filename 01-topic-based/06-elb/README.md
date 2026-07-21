Excellent! Before moving to **IAM**, here is a compact summary of **Elastic Load Balancing (ELB)** that covers almost everything you need for the SAA exam.

---

# ELB (Elastic Load Balancing) — SAA Summary

## 1. Load Balancer Types

| Load Balancer                       | Layer     | Best For                                   |
| ----------------------------------- | --------- | ------------------------------------------ |
| **Application Load Balancer (ALB)** | Layer 7   | HTTP/HTTPS, Web apps, Microservices        |
| **Network Load Balancer (NLB)**     | Layer 4   | TCP/UDP/TLS, High performance, Low latency |
| **Gateway Load Balancer (GWLB)**    | Layer 3/4 | Firewalls, IDS/IPS, Security appliances    |
| **Classic Load Balancer (CLB)**     | Legacy    | Older applications                         |

---

# 2. When to Use ALB ⭐⭐⭐

Choose **ALB** when you see:

* HTTP / HTTPS
* Web application
* REST API
* Microservices
* ECS
* EKS
* Lambda targets
* Host-based routing
* Path-based routing
* gRPC
* WebSockets

---

# 3. ALB Features ⭐⭐⭐

### Path-Based Routing

Example:

```text
/api/android  → Android Target Group
/api/ios      → iOS Target Group
```

Keyword:

> URL Path

---

### Host-Based Routing

Example:

```text
api.company.com
shop.company.com
```

Keyword:

> Host Header

---

### gRPC Support

Only

> **ALB**

supports

* HTTP/2
* gRPC
* Bidirectional streaming

---

# 4. Cross-Zone Load Balancing ⭐⭐⭐

Without Cross-Zone

Each Load Balancer node sends traffic only to targets in its own AZ.

Can create uneven traffic.

---

With Cross-Zone

Every Load Balancer node sends traffic to **all healthy targets in every enabled AZ**.

Result

✅ Even distribution

---

Remember

ALB

> Always Enabled

NLB

> Disabled by default

GWLB

> Disabled by default

---

# 5. Health Checks ⭐⭐⭐

ALB uses

* HTTP
* HTTPS

NLB uses

* TCP
* HTTP
* HTTPS

CLB uses

* TCP
* HTTP
* HTTPS
* SSL

---

If ALB shows

```text
OutOfService
```

Think

> Health Check Failure

Common causes

* Wrong path
* Wrong port
* App not running
* Wrong success code
* Timeout

---

# 6. ALB Access Logs ⭐⭐⭐

Enable

> Access Logs

Stored in

> Amazon S3

Contains

* Client IP
* Request URL
* Latency
* Response code
* Target processing time

---

Remember

CloudTrail

Logs

AWS API calls

NOT

HTTP requests

---

# 7. CloudWatch Application Insights

Used for

* Monitoring
* Troubleshooting
* Automatic dashboards
* Detecting anomalies
* Reducing MTTR

Often paired with

ALB Access Logs

---

# 8. AWS WAF ⭐⭐⭐⭐⭐

Protects

* ALB
* CloudFront
* API Gateway
* AppSync

Protects against

* SQL Injection
* Cross-Site Scripting (XSS)
* Bad Bots
* HTTP attacks
* Rate limiting

---

### WAF Regular Rule

Blocks

* IP
* URI
* Headers
* Country
* Query strings

---

### WAF Rate-Based Rule ⭐⭐⭐

Keywords

* Too many requests
* Excessive requests
* Brute force
* DDoS-like traffic

Answer

> Rate-Based Rule

---

# 9. WAF vs Firewall Manager

### AWS WAF

Actually blocks traffic.

---

### Firewall Manager

Manages

* WAF
* Shield
* Security Groups

Across

Multiple AWS Accounts

Think

> Firewall Manager manages WAF.

---

# 10. WAF vs NACL

WAF

* Layer 7
* HTTP inspection
* SQL Injection
* XSS

NACL

* Layer 3/4
* IP
* Port
* Protocol

Cannot inspect HTTP payloads.

---

# 11. AWS X-Ray

Used for

* Distributed tracing
* Performance analysis
* Debugging

NOT

* SQL Injection protection
* XSS protection

---

# 12. Global Accelerator

Purpose

Improve

Global application performance

Uses

AWS Global Network

Does NOT

* Replace ALB
* Perform path routing
* Support gRPC routing

---

# 13. Route 53 vs ELB

Route 53

Routes users

↓

Load Balancer

Load Balancer

↓

Routes requests

↓

EC2 Targets

---

# Decision Table

| Requirement                  | Service                         |
| ---------------------------- | ------------------------------- |
| HTTP / HTTPS                 | ALB                             |
| TCP / UDP                    | NLB                             |
| Firewall Appliance           | GWLB                            |
| Path Routing                 | ALB                             |
| Host Routing                 | ALB                             |
| gRPC                         | ALB                             |
| Health Check (ALB)           | HTTP / HTTPS                    |
| Health Check (NLB)           | TCP                             |
| Client IP & Request Logs     | ALB Access Logs                 |
| AWS API Logs                 | CloudTrail                      |
| SQL Injection                | AWS WAF                         |
| XSS                          | AWS WAF                         |
| Too Many Requests            | WAF Rate-Based Rule             |
| Equal Traffic Across AZs     | Cross-Zone Load Balancing       |
| Multi-Account WAF Management | Firewall Manager                |
| Application Tracing          | AWS X-Ray                       |
| Monitoring & Troubleshooting | CloudWatch Application Insights |

---

# Exam Keywords → Immediate Answer

| Keyword                       | Answer                    |
| ----------------------------- | ------------------------- |
| URL Path                      | ALB                       |
| Host Header                   | ALB                       |
| gRPC                          | ALB                       |
| HTTP Request Logs             | ALB Access Logs           |
| AWS API Logs                  | CloudTrail                |
| SQL Injection                 | AWS WAF                   |
| XSS                           | AWS WAF                   |
| Too Many Requests             | WAF Rate-Based Rule       |
| OutOfService                  | Health Check              |
| Even Traffic Across AZs       | Cross-Zone Load Balancing |
| Firewall Appliance            | GWLB                      |
| Ultra-low latency / TCP / UDP | NLB                       |

---

## Final Memory Rules (Very High Yield)

* **ALB = Layer 7 = Web Applications**
* **NLB = Layer 4 = TCP/UDP + High Performance**
* **GWLB = Security Appliances**
* **Path Routing / Host Routing / gRPC = ALB**
* **Cross-Zone = Even traffic across AZs**
* **Target OutOfService = Check Health Check first**
* **Access Logs = HTTP request details (stored in S3)**
* **CloudTrail = AWS API activity**
* **SQL Injection / XSS = AWS WAF**
* **Too many requests = WAF Rate-Based Rule**
* **Firewall Manager manages WAF across multiple accounts**
* **X-Ray = Tracing, not security**

---

You're now ready to move on to **IAM**, which is another core SAA topic. Understanding IAM well will help not only in the exam but also in real-world AWS architecture and DevOps work.


21-July-2026
