
Haan, bilkul! **AWS Instance Scheduler** asal mein ek pre-made CloudFormation template hota hai jise AWS khud provide karta hai.

Jab aap is template ko deploy karte hain, toh yeh aapke account mein EventBridge rules, Lambda functions, aur DynamoDB tables create kar deta hai. Is ke zariye aap easily tags (jaise `Schedule: office-hours`) ke thor par **EC2 aur RDS instances ko automatically fixed timing par ON aur OFF (start/stop)** kar sakte hain.

---

### Key Takeaway for AWS Exam:

* **Working Hours / Scheduled Stop-Start:** Jab bhi question mein EC2/RDS ko fixed business hours par stop/start karke cost save karne ka poocha jaye, toh **AWS Instance Scheduler** sab se best, automated aur minimal effort wala solution hota hai.


---
---
---

**Question Kya Kah Raha Hai?**
Company apna **on-premises MySQL database** AWS par migrate karna chahti hai. Application par traffic unpredictable hai (kabhi sudden spikes aate hain, toh kabhi zero traffic hota hai). Unhe ek aisa managed database chahiye jo:

1. **MySQL-compatible** ho.
2. **Serverless Capacity Scaling** ke zariye traffic ke mutabiq automatically scale-up aur scale-down ho sake.

---

**Correct Answer:**
**Configure an Amazon Aurora Serverless v2 database with a minimum capacity of 1 and a maximum of 8 Aurora capacity units (ACUs).**

---

**Key Concepts & Exam Elimination Rules:**

1. **MySQL Compatibility + Serverless:** On-prem MySQL ko as-is replace karne ke liye **Aurora Serverless v2** sabse best fit hai kyunki yeh native MySQL-compatible hai aur unpredictable workloads par milliseconds mein scale karti hai.
2. **ACU Math (Aurora Capacity Units):** 1 ACU $\approx$ 2 GiB RAM. Question mein 2 se 16 GiB RAM ki requirement hai, isliye 1 ACU (2 GiB) se 8 ACU (16 GiB) ki range exact fit baithti hai.
3. **DynamoDB Kyun Galat Hai?** DynamoDB ek **NoSQL** database hai, jabki requirement existing **MySQL (Relational)** DB ko migrate karne ki hai.
4. **Provisioned RDS / Aurora Instance Kyun Galat Hai?** Fixed instance classes fluctuating/zero traffic par auto-scaling serverless capacity demand poori nahi kar sakte aur zero activity par bhi fixed cost charge karte rehte hain.



03-October-2026
