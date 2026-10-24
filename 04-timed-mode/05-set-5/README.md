
* **AWS DataSync:** Kyunke on-premises storage khatam ho rahi thi aur millions of obsolete records ko *foran* AWS par shift karna tha, iske liye **AWS DataSync** sab se fast aur secure online data transfer/migration service hai. (Storage Gateway tab use hota hai jab data on-prem rakhte hue hybrid access chahiye ho, lekin yahan data ko poori tarah move karna tha).




* **CloudTrail Data Events:** Sawal mein pucha gaya hai ke **"each data access must be audited at all levels"**. CloudTrail ke andar **Management Events** sirf bucket creation ya settings change (control plane) ko track karte hain, jabki individual objects par hone wali data access (jaise `GetObject` ya `PutObject`) ko track karne ke liye **Data Events** enable karne parte hain.

---
---
---


### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **Amazon Aurora:** Yeh AWS ka high-performance, fully managed relational database service hai jo MySQL aur PostgreSQL ke sath compatible hai.
* Yeh **OLTP** workloads aur **ACID compliance** ko mukammal support karta hai.
* Sab se main wajah yeh hai ke data pehle hi **50 TB** hai aur mazeed barhega. Standard databases kemukabilay, **Aurora automatically storage ko scale karta hai (upto 128 TB)** baghair kisi manual intervention ke, jo is massive scale ke liye perfect hai.



23-October-2026

24-October-2026
