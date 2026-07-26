Congratulations! 🎉 You've finished one of the biggest topics in the SAA exam. Before moving to **CloudFront**, here's a concise VPC summary that covers almost every exam trick you've encountered.

---

# Amazon VPC Summary

## 1. What is a VPC?

A **Virtual Private Cloud (VPC)** is your own isolated virtual network inside AWS where you launch resources like EC2, RDS, and Load Balancers.

Think of it as your own private data center in the AWS cloud.

---

# 2. Public vs Private Subnet

### Public Subnet

* Has a route to an **Internet Gateway (IGW)**
* Can host:

  * Web servers
  * Bastion Hosts
  * NAT Gateway
  * ALB (Internet-facing)

Example:

```
Internet
    │
Internet Gateway
    │
Public Subnet
    │
EC2
```

---

### Private Subnet

* No direct Internet access
* Used for:

  * Databases
  * Backend servers
  * Internal applications

Example:

```
Internet

×

Private Subnet

EC2

RDS
```

---

# 3. Internet Gateway (IGW)

Purpose:

Allows resources in **public subnets** to communicate with the Internet.

Remember:

> Public subnet = Route Table → Internet Gateway

---

# 4. NAT Gateway

Purpose:

Allows **private subnet** resources to **access the Internet**, but blocks inbound Internet connections.

Example:

```
Private EC2

↓

NAT Gateway

↓

Internet Gateway

↓

Internet
```

Used for:

* yum update
* apt update
* Download packages
* Access AWS APIs

---

# 5. Security Groups (SG)

Acts like a firewall for an **EC2 instance**.

Properties:

* Stateful
* Allow rules only
* Instance level

If inbound is allowed, return traffic is automatically allowed.

---

# 6. Network ACL (NACL)

Acts like a firewall for a **subnet**.

Properties:

* Stateless
* Allow AND Deny rules
* Subnet level

Need to allow:

* Incoming port
* Outgoing ephemeral ports

---

# SG vs NACL

| Security Group           | NACL                       |
| ------------------------ | -------------------------- |
| Instance level           | Subnet level               |
| Stateful                 | Stateless                  |
| Allow only               | Allow & Deny               |
| Return traffic automatic | Must allow both directions |

---

# 7. VPC Peering

Allows two VPCs to communicate privately.

Requirements:

* Non-overlapping CIDR blocks
* Route tables updated

---

## Exam Rule

VPC Peering is

❌ NOT transitive

If

```
A ←→ B ←→ C
```

A cannot talk to C.

---

Also remember:

No edge-to-edge routing.

Cannot share:

* Internet Gateway
* NAT Gateway
* VPN
* Direct Connect
* Gateway Endpoint

---

# 8. Site-to-Site VPN

Connects

```
On-Prem

↓

Customer Gateway

↓

Internet (Encrypted)

↓

Virtual Private Gateway

↓

AWS VPC
```

Need:

* Customer Gateway
* Virtual Private Gateway
* Static public IP on customer side

---

# 9. Direct Connect

Private dedicated network

```
On-Prem

↓

Direct Connect

↓

AWS
```

Benefits:

* Faster
* Lower latency
* More reliable

Often paired with VPN for backup.

---

# 10. Customer Gateway

Lives on-premises.

Represents:

Your router/firewall.

---

# 11. Virtual Private Gateway

Lives in AWS.

Attached to a VPC.

Used for:

* Site-to-Site VPN
* Direct Connect

---

# 12. Gateway Endpoint

Supports ONLY

* S3
* DynamoDB

Benefits:

* Free
* Private AWS network
* Uses Route Table

---

# 13. Interface Endpoint (PrivateLink)

Supports:

* SNS
* SQS
* Lambda
* CloudWatch
* KMS
* Secrets Manager
* Many other AWS services

Creates an ENI inside your subnet.

Costs money.

---

# 14. IPv4 vs IPv6

AWS now supports:

* IPv4
* IPv6
* Dual Stack
* IPv6-only subnets

Remember:

You cannot create an IPv6-only VPC.

The VPC always has IPv4.

---

# 15. Route Tables

Route tables decide

> Where should packets go?

Examples:

```
0.0.0.0/0

↓

Internet Gateway
```

or

```
0.0.0.0/0

↓

NAT Gateway
```

or

```
S3 Prefix

↓

Gateway Endpoint
```

---

# Exam Tricks

### If S3 must stay private

✅ Gateway Endpoint

---

### If SNS/SQS must stay private

✅ Interface Endpoint

---

### Private subnet needs Internet

✅ NAT Gateway

---

### Public website

✅ Internet Gateway

---

### Connect AWS to office

✅ Site-to-Site VPN

---

### Faster dedicated office connection

✅ Direct Connect

---

### Connect two VPCs

✅ VPC Peering

---

### Three VPCs

```
A ←→ B ←→ C
```

A cannot reach C.

---

### EC2 can't access website

First check:

✅ Security Group (HTTP/HTTPS)

Then:

✅ Route Table

---

### NACL Question

Remember:

Allow

* Service port inbound
* Ephemeral ports outbound

---

# Final Cheat Sheet

| Requirement                    | AWS Service             |
| ------------------------------ | ----------------------- |
| Public Internet access         | Internet Gateway        |
| Private subnet Internet access | NAT Gateway             |
| EC2 firewall                   | Security Group          |
| Subnet firewall                | Network ACL             |
| Connect two VPCs               | VPC Peering             |
| Connect office to AWS          | Site-to-Site VPN        |
| Dedicated private connection   | Direct Connect          |
| Private S3 access              | Gateway Endpoint        |
| Private SNS/SQS access         | Interface Endpoint      |
| Office router                  | Customer Gateway        |
| AWS VPN endpoint               | Virtual Private Gateway |

---

## Ready for CloudFront

CloudFront is generally much easier than VPC because it focuses on content delivery rather than networking. The main concepts you'll learn are:

* **CloudFront as a CDN** (bringing content closer to users)
* **Origins** (S3 buckets, ALBs, EC2, or custom servers)
* **Edge Locations**
* **Caching behavior**
* **Signed URLs and Signed Cookies**
* **Origin Access Control (OAC)** for securing S3 origins
* **HTTPS with ACM**
* **CloudFront + S3** (one of the most common SAA architectures)

After CloudFront, you'll have covered many of the highest-frequency SAA services.


26-July-2026
