Bohot hi umda sawal! Jab tak sawal ke andar ke **technical terms** bilkul clear na hon, tab tak concept pakka nahi hota. Aaiye in teeno terms ko bilkul aasan real-world examples se samajhte hain:

---

### 1. GIS Kya Hai? (Geographic Information Systems) 🗺️

* **Asan Alfaz mein:** GIS ek aisa system hota hai jo **maps, zameen ki tasweeron (satellite images), aur location data** ko handle, store aur analyze karta hai.
* **Real-world Example:** Sochiye aap ke paas ek aesa software hai jo Pakistan ya poori duniya ki satellite live tasweerein dikhata hai ke kahan barish ho rahi hai, kahan flood (sailab) aane wala hai, ya kahan aag lagi hai. Yeh sab **GIS** kehlata hai.

---

### 2. Geographic Code Kya Hai? (Geographic Code) 📍

* **Asan Alfaz mein:** Jaise har insaan ka ek CNIC number hota hai aur har ghar ka ek postal address hota hai, waise hi GIS maps par **har specific jagah (area) ka ek unique ID code** hota hai (misal ke taur par: `PK-GUJ-001` ya koi coordinate code).
* **Sawal mein iska kya kaam hai:** Jab satellite koi tasweer leta hai, toh uske sath yeh code laga hota hai taake pata chal sake ke yeh tasweer kis ilaqay ki hai.

---

### 3. Single-Table Design Kya Hai? 🗂️

* **Asan Alfaz mein:**
* Aam relational databases (jaise MySQL) mein data alag-alag tables mein baanta jata hai (ek table users ki, ek orders ki, phir unhein `JOIN` kiya jata hai).
* Lekin **Single-Table Design** NoSQL databases (jaise DynamoDB) mein use hota hai, jahan **saari related information sirf ek hi bari table ke andar** rakhi jati hai taake data dhoondne mein aik second ka bhi delay na ho.


* **Is sawal mein:** Tasweer ka code aur uska link sab kuch ek hi simple table mein save hai.

---

### Sawal Ka Poora Scene Ab Kya Banta Hai? 🔄

Ab aap poori kahani samajh gaye hain:

1. System ke paas **bohot heavy high-resolution satellite images** hain (jo GIS ke zariye aa rahi hain).
2. Har image ke sath ek **Geographic Code** juda hua hai.
3. Masla yeh hai ke heavy images ko database mein rakhna mehenga parta hai, isliye hum **images ko Amazon S3 (sasta storage)** mein rakh dete hain, aur unka **Geographic Code aur S3 ka link ek single DynamoDB table** mein save kar lete hain!

---
---
---

**Bilkul 100% Sahi!** 🎯

Aap ne poore concept ko ek hi line mein master kar liya hai:

* **Heavy Images ➔ Amazon S3** (Object storage jo sasta hai aur unlimited capacity deta hai).
* **Geographic Code + S3 Image Link ➔ Amazon DynamoDB** (Fast NoSQL table jo lightning-speed lookups deta hai).

Yeh AWS Cloud Architecture ka sab se mashhoor aur standard **Storage & Database Decoupling Pattern** hai.

---
---
---
17. Question

An advertising company is currently working on a proof of concept project that automatically provides SEO analytics for its clients. Your company has a VPC in AWS that operates in a dual-stack mode in which IPv4 and IPv6 communication is allowed. You deployed the application to an Auto Scaling group of EC2 instances with an Application Load Balancer in front that evenly distributes the incoming traffic. You are ready to go live but you need to point your domain name (tutorialsdojo.com) to the Application Load Balancer.

In Route 53, which record types will you use to point the DNS name of the Application Load Balancer? (Select TWO.)

- Non-Alias with a type "A" record set
- Alias with a type "A" record set
- Alias with a type "CNAME" record set
- Alias with a type "AAAA" record set
- Alias with a type of “MX” record set

Yes. The easiest way is to think of **A/AAAA = actual address**, while **CNAME/ALIAS = point to another name/resource**.

### 1. A Record — IPv4

```text
example.com → 192.0.2.10
```

**A = IPv4 address**

---

### 2. AAAA Record — IPv6

```text
example.com → 2001:db8::10
```

**AAAA = IPv6 address**

---

### 3. CNAME — Name → Another Name ⭐

CNAME does **not** point directly to an IP.

Example:

```text
www.example.com
       ↓
app.example.com
       ↓
192.0.2.10
```

You create:

```text
www.example.com CNAME app.example.com
```

Meaning:

> "If someone asks for `www.example.com`, use `app.example.com`."

### Real-world example

Suppose AWS gives you:

```text
myapp.us-east-1.elb.amazonaws.com
```

You want users to use:

```text
www.example.com
```

You can create:

```text
www.example.com → CNAME → myapp.us-east-1.elb.amazonaws.com
```

**Shortcut:**  
**CNAME = domain name → another domain name**

---

# 4. Alias Record — AWS-specific ⭐⭐⭐

An **Alias record** in Route 53 points your domain to certain **AWS resources**.

Example:

```text
www.example.com
       ↓
Alias
       ↓
Application Load Balancer
```

Or:

```text
example.com
     ↓
Alias
     ↓
CloudFront distribution
```

Or:

```text
example.com
     ↓
Alias
     ↓
S3 website endpoint
```

The important difference:

**Alias can point to AWS resources directly.**

---

## CNAME vs Alias — VERY IMPORTANT for SAA

| | CNAME | Alias |
|---|---|---|
| Points to | Another hostname | AWS resource |
| IP address directly? | ❌ | AWS handles it |
| Route 53 feature? | DNS standard | AWS Route 53 feature |
| Root domain (`example.com`)? | ❌ Generally no | ✅ Yes |
| ALB/CloudFront? | Can point to hostname | ✅ Direct AWS integration |

### 🔥 Exam shortcut

If you see:

> **Route 53 + ALB/CloudFront + root domain**

Think:

**Alias record**

Example:

```text
example.com
     ↓
Route 53 Alias
     ↓
ALB
```

If you see:

> **One domain name needs to point to another domain name**

Think:

**CNAME**

```text
www.example.com
       ↓
CNAME
       ↓
app.example.com
```

### 🧠 Remember all four

```text
A       → IPv4
AAAA    → IPv6
CNAME   → Name → Name
ALIAS   → Name → AWS resource
```

**Most important SAA trick:**  
**Root/apex domain + AWS resource → Alias.**

---
---
---

Exactly! ✅

Your mental model is correct:

- **Domain → another domain/hostname** → **CNAME**
- **Domain → AWS resource** → **Alias**

Examples:

```text
www.example.com
      ↓
CNAME
      ↓
app.example.com
```

```text
www.example.com
      ↓
Alias
      ↓
Application Load Balancer
```

### 🔥 SAA shortcut
**Name → Name = CNAME**  
**Name → AWS resource = Alias**

And one very important exam point:

**Root domain (`example.com`) → ALB/CloudFront → Alias**, because a CNAME generally cannot be used at the DNS zone apex.

---
---
---

**Bilhul 100% Sahi pakra hai aap ne!** 🎯

Aap ne poori baat ko bilkul ek line mein sum up kar diya:

* **Domain to another domain (sub-domain level):** ➔ **CNAME**
* **Domain/Root to an AWS Resource (ALB, S3, CloudFront, etc.):** ➔ **Alias**

Yeh rule yaad rakh liya toh Route 53 ke exam questions mein kabhi ghalat nahi hoga!

---
---
---

Yes — this question is actually testing one very specific **AWS Glue feature**.

### 🎯 What is the question saying?

The bank has:

```text
S3
 ↓
XML financial data
```

Every day, **new data is added**.

But their Glue ETL job does this:

```text
Day 1:
100 files → process all 100

Day 2:
20 new files added

Glue processes:
120 files ❌
```

So Glue is **processing old data again and again**.

The bank wants:

```text
Day 1:
100 files → process 100

Day 2:
20 new files → process ONLY 20 ✅

Day 3:
10 new files → process ONLY 10 ✅
```

This saves **processing time and resources**.

---

### ✅ Correct answer: Enable job bookmarks

**AWS Glue Job Bookmarks** allow Glue to **remember what data it has already processed**.

Think of a bookmark like a **bookmark in a book** 📖.

If you read pages 1–100 today, you put a bookmark at page 100.

Tomorrow:

> "Continue from where I stopped."

Similarly:

```text
First run:
S3 files 1–100
       ↓
Glue processes them
       ↓
Bookmark remembers them

Next run:
S3 files 1–100 + 101–120
                  ↑
          Process only new data
```

So:

> **Job Bookmark = remembers previously processed data and helps process only new/changed data.**

---

### ❌ Why the other answers?

**Glue Data Quality**

Used to check whether data is **valid/accurate/complete**.

Example:

> Is `customer_id` present?  
> Is age within an expected range?

It doesn't solve repeated processing.

---

**FindMatches ML Transform**

Used to find **matching/duplicate records**.

Example:

```text
John Smith, 123 Main St
John Smith, 123 Main Street
```

It can identify that they may represent the same person.

Not for tracking previously processed files.

---

**NumberOfWorkers = 3**

Only changes the **amount of compute capacity**.

It doesn't tell Glue:

> "Don't process the old files."

---

### 🔥 SAA shortcut

When you see:

> **Glue keeps processing the same old data + want only new/changed data**

Immediately think:

**👉 Glue Job Bookmarks** ✅

### 🧠 One-line memory

**Job Bookmark = "Glue, remember what you already processed."**

---
---
---

### 🧠 One-Line Memory

> **Stateless App + Traffic stuck on 1 EC2 instance ➔ Disable ALB Sticky Sessions**


29-October-2026

30-October-2026

31-October-2026
