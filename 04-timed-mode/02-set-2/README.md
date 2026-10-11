### Keywords Scan 🔍

1. **"temporary / short-lived access tokens that act as temporary security credentials"**
* **Trigger:** AWS STS (**Security Token Service**).


2. **"leveraging AWS Identity and Access Management (IAM) roles, to allow access to AWS resources across accounts"**
* **Trigger:** Cross-account access using `AssumeRole` via **AWS STS**.



---

### Correct Answer

**Use AWS Security Token Service (STS)**

---

### Roman Urdu Explanation 💡

* **AWS Security Token Service (AWS STS):**
* STS ek AWS service hai jo IAM users ya federated users ko temporary, short-lived credentials (Access Key ID, Secret Access Key, aur Session Token) provide karti hai.
* Jab junior developer ko temporary cross-account access chahiye hota hai, toh woh STS ka `AssumeRole` API call karta hai.
* STS use **15 minutes se 12 hours** tak ka temporary session token deta hai. Token expire hotay hi access automatic khatam ho jata hai, jo security clearance ke liye ideal solution hai.



---

### Elimination Rules ❌

* **Amazon Cognito:** User authentication (login/signup) aur JWT tokens mobile/web apps ke end-users ke liye handle karta hai, junior developer ke IAM cross-account access ke liye nahi.
* **AWS IAM Identity Center:** Single Sign-On (SSO) aur workforce access manage karta hai, lekin **short-lived temporary credentials and tokens generate karne ki core underlying service AWS STS hai**.
* **All of the given options are correct:** Cognito aur IAM Identity Center exact programmatically issued short-lived tokens / AssumeRole API mechanics direct solve nahi karte, isliye yeh option ghalt hai.

---
---
---

**BOHUT BARIYA OBSERVATION!** 👏 Aap ne bilkul sahi point pakda!

Boht se log yeh samajhte hain ke **AWS DMS (Database Migration Service)** sirf *On-Premises to AWS* data move karne ke liye hoti hai, lekin yeh bilkul sach nahi hai.

---

### AWS DMS Kahan Kahan Kaam Kar Sakti Hai? 🔄

AWS DMS 3 types ke scenarios mein use hoti hai:

| Migration Type | Explanation | Real-World Example |
| --- | --- | --- |
| **On-Premises ➔ AWS** | Physical Data Center se Cloud par data lana. | On-prem MySQL ➔ Amazon RDS |
| **AWS ➔ AWS (Cloud-to-Cloud)** | Cloud ke andar hi ek service se doosri service par switch karna. | **EC2 Self-Hosted DB ➔ Amazon DynamoDB** |
| **AWS ➔ On-Premises** | AWS Cloud se data wapas local servers par lana. | Amazon Aurora ➔ Local PostgreSQL |

---

### Is Question Mein DMS Cloud-to-Cloud Kyun Use Hui? 💡

Is question mein target kya tha?

* **Source:** EC2 Instance ke andar chalne wala embedded NoSQL DB.
* **Target:** Fully Managed **Amazon DynamoDB**.

Agar aap live app chalate waqt database change karenge, toh data loss ya application downtime ho sakta hai.

**AWS DMS** ka **CDC (Change Data Capture / Ongoing Replication)** feature yeh karta hai:

1. Yeh EC2 ke embedded DB se sara purana data DynamoDB mein copy karta hai.
2. Jab tak app chal rahi hoti hai, jo bhi naya write request EC2 DB par aata hai, DMS usko real-time mein DynamoDB mein sync karta rehta hai.
3. Jab dono DBs 100% sync ho jaate hain, aap app ko DynamoDB par point karke purana EC2 DB band kar dete hain **(Zero Downtime Migration!)**.

---

### Summary Rule 📌

> **AWS DMS** ka matlab sirf "On-Prem to Cloud" nahi hai. **AWS Cloud ke andar hi** ek DB (e.g. EC2 par self-hosted MongoDB/MySQL) se doosri DB service (e.g. DynamoDB/RDS) par shift hone ke liye bhi DMS sab se primary tool hai!

---



10-October-2026
