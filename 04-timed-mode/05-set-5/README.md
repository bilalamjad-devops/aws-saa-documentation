
* **AWS DataSync:** Kyunke on-premises storage khatam ho rahi thi aur millions of obsolete records ko *foran* AWS par shift karna tha, iske liye **AWS DataSync** sab se fast aur secure online data transfer/migration service hai. (Storage Gateway tab use hota hai jab data on-prem rakhte hue hybrid access chahiye ho, lekin yahan data ko poori tarah move karna tha).




* **CloudTrail Data Events:** Sawal mein pucha gaya hai ke **"each data access must be audited at all levels"**. CloudTrail ke andar **Management Events** sirf bucket creation ya settings change (control plane) ko track karte hain, jabki individual objects par hone wali data access (jaise `GetObject` ya `PutObject`) ko track karne ke liye **Data Events** enable karne parte hain.

---
---
---


### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **Amazon Aurora:** Yeh AWS ka high-performance, fully managed relational database service hai jo MySQL aur PostgreSQL ke sath compatible hai.
* Yeh **OLTP** workloads aur **ACID compliance** ko mukammal support karta hai.
* Sab se main wajah yeh hai ke data pehle hi **50 TB** hai aur mazeed barhega. Standard databases kemukabilay, **Aurora automatically storage ko scale karta hai (upto 128 TB)** baghair kisi manual intervention ke, jo is massive scale ke liye perfect hai.

---
---
---

### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **Short Polling vs. Long Polling:**
* **Short Polling (`ReceiveMessageWaitTimeSeconds` = 0):** Isme SQS foran return kar deta hai, chahe queue khali ho. Is se bohot saari empty responses generate hoti hain jo API calls aur CPU cycles waste karti hain.
* **Long Polling (`ReceiveMessageWaitTimeSeconds` > 0):** Isme SQS wait karta hai (maximum 20 seconds tak) ke jab tak koi message queue mein na aa jaye. Agar message nahi hota, toh yeh foran reply karne ke bajaye thori der wait karta hai, jisse empty responses ki tadad dramatic tareeqay se kam ho jati hai, CPU waste nahi hota, aur cost bhi bachti hai.

---
---
---

### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **AWS Network Firewall:** Yeh ek fully managed service hai jo aap ko apne VPCs par network protections deploy karne ki ijazat deti hai. Iske zariye aap custom rule groups (Stateless aur Stateful rules) bana kar VPC ke andar aane wale (ingress) aur bahar jane wale (egress) traffic ko live inspect aur filter kar sakte hain. Isko implement karne ke liye VPC ke route tables ko update kiya jata hai taake traffic firewall se hokar guzrey.

---

43. Question

A startup launched a fleet of on-demand Amazon EC2 instances to host a massively multiplayer online role-playing game (MMORPG). The EC2 instances are configured with Auto Scaling and AWS Systems Manager. 

What can be used to configure the EC2 instances without having to establish an RDP or SSH connection to each instance?

- AWS CodePipeline
- `Run Command`
- EC2Config
- AWS App Runner

---
---
---

### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **AWS KMS (Key Management Service):** Amazon EBS ki encryption mukammal tor par **AWS KMS** par depend karti hai.
* Aap ya toh AWS ki taraf se di gayi default key (**Amazon-managed key**, jiska alias `aws/ebs` hota hai) use kar sakte hain.
* Ya phir apni khud ki custom keys (**Customer-managed keys / your own keys**) KMS mein create kar ke EBS volume encryption ke liye select kar sakte hain.


23-October-2026

24-October-2026
