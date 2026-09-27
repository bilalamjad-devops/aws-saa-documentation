Bilkul! Aap ne Practice Set 5 ke **1 se 65 tak ke saare questions complete kar liye hain**.

Aayein pure Practice Set 5 ki ek concise aur solid **Exam-Focused Summary Sheet** dekhte hain jo aap ke SAA-C03 exam review ke liye boht kaam aayegi:

---

## 📌 Practice Set 5 Key Concepts Cheat Sheet

### 🌐 1. Route 53 & Hybrid Migration

* **Route 53 Record Types:**
* **A Record:** Domain ko static IPv4 address par map karta hai.
* **CNAME Record:** Subdomain ko doosre domain name par map karta hai (Zone Apex / Root Domain par disallowed).
* **Alias A Record:** Route 53 ka feature jo Zone Apex ko dynamic AWS resources (ALB, CloudFront, S3) par map kar sakta hai.


* **Traffic Splitting (50/50 Hybrid Migration):**
* **Route 53 Weighted Routing:** DNS level par percentage basis par traffic split karta hai.
* **ALB with Weighted Target Groups:** Load balancer level par Direct Connect / IP targets ke zariye traffic split karta hai.



---

### 🗄️ 2. Database Failover & Storage Mechanics

```
┌─────────────────────────────────────────────────────────────────┐
│                      AURORA FAILOVER LOGIC                      │
├─────────────────────────────────────────────────────────────────┤
│  Has Read Replica?  ──► YES ──► Promote Replica + Flip CNAME     │
│                     ──► NO  ──► Re-create Instance (Same AZ)    │
└─────────────────────────────────────────────────────────────────┘

```

* **Standard RDS Multi-AZ:** Dedicated passive **Standby Instance** hota hai jo Failover par CNAME swap ke zariye active banta hai.
* **Amazon Aurora:**
* **With Read Replica:** Aurora **Read Replica** ko promote karta hai aur Cluster Endpoint ka **CNAME flip** karta hai.
* **Single Instance (No Replica):** Promote hone ke liye koi target nahi hota, isliye Aurora **same AZ mein naya DB instance recreate** karta hai (best-effort basis par).



---

### 🛡️ 3. Web Filtering & Security Automation

* **AWS WAF Geo-Blocking + Whitelisting Priority:**
1. **Priority 1 (High):** **Allow Rule** (IP Set for approved specific IPs).
2. **Priority 2 (Low):** **Block Rule** (Geo Match Condition for country-wide blocking).


* **AWS CloudHSM Security:** 3 failed admin login attempts par **Zeroize** (wipe out) ho jata hai. Unrecoverable without backups.

---

### ⚙️ 4. Compute, Scaling & Networking

* **Auto Scaling Policies:**
* **Target Tracking:** Target metric ko constant rakhta hai (e.g., maintain average CPU at 50%).
* **Step Scaling:** CloudWatch Alarms ke multiple thresholds aur **step ranges** ke mutabiq scaling karta hai.
* **Scheduled Scaling:** Pre-determined time/date par scaling karta hai.


* **SQS Long Polling:** `ReceiveMessageWaitTimeSeconds > 0` (max 20s) set karne se empty responses khatam hote hain aur API costs reduce hoti hain.
* **VPN Scaling:** Transit Gateway (TGW) + Equal-Cost Multi-Path Routing (ECMR) use karke standard 1.25 Gbps VPN tunnel limit ko scale kiya jata hai.
* **Service Limits Monitoring:** AWS Trusted Advisor + Lambda + EventBridge + SNS pipeline ke zariye automated quota tracking hoti hai.

---

24-September-2026

27-September-2026
