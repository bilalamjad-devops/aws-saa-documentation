#### Answer Kya Keh Raha Hai? 💡

* **Expedited Retrieval:** S3 Glacier mein 3 kisam ki retrieval options hoti hain:
1. *Bulk Retrieval:* 5–12 hours lagte hain (sab se sasta).
2. *Standard Retrieval:* 3–5 hours lagte hain.
3. *Expedited Retrieval:* **1–5 minutes** mein data mil jata hai (jo 15 minutes ki requirement ko easily meet karta hai).


* **Provisioned Capacity:** Expedited retrievals normally best-effort basis par hoti hain (yani agar S3 par heavy load ho toh delay aa sakta hai). Lekin jab aap **Provisioned Capacity** purchase kar lete hain, toh AWS guarantee deta hai ke aap ki Expedited retrieval capacity har waqt available rahegi aur high throughput (up to 150 MB/s) support karegi.

**Expedited** (ya verbs: *expedite*) ka lafzi matlab hota hai: **"Tez tar karna"**, **"Jaldi niptana"**, ya **"Foori taur par rawana karna"**.

---

### Lafzi Maani & Real-Life Usages 📝

* **Expedite (Verb):** Kisi kaam ki raftaar ko barhana taake wo time se pehle ya foori taur par ho jaye (e.g., *"Please expedite my passport processing."* — *"Meharbani karke mera passport jaldi banwa dein."*).
* **Expedited (Adjective):** Ek aisa process jo normal se zyada **Fast-Track / Urgent** ho.

---

### Real-World Analogy 🚚✈️

| Normal Process (Standard) | Expedited Process (Tez/Urgent) |
| --- | --- |
| Normal Courier/Postage (Jo 5 se 7 din mein parcel pahunchata hai). | **Urgent / Urgent Express Delivery** (Jo extra charges le kar 24 ghante mein delivery deta hai). |

---

### AWS S3 Glacier Context ☁️⚡

AWS S3 Glacier mein **"Expedited Retrieval"** ka matlab hi yahi hai ke yeh aap ki data nikalne ki request ko **Fast-Track (Urgent)** lane mein daal deta hai, taake 3-5 ghante (Standard Time) ke bajaye sirf **1 se 5 minutes** mein data mil jaye!

---
---
---


#### Answer Kya Keh Raha Hai? 💡

1. **Instance Store Data Loss:** Instance Store volumes **ephemeral (temporary)** hotay hain. Jab instance stop hota hai, wo jis physical hypervisor server par chal raha hota hai us se detached ho jata hai, aur uska sara data permanantly delete (erase) ho jata hai.
2. **Underlying Host Change:** Stop/Start karne se AWS instance ko kisi naye physical host server par launch kar sakta hai (AWS underlying physical host machine change kar deta hai).

---
---
---

### Comparison Table 📊

| Feature | Active-Active | Active-Passive |
| --- | --- | --- |
| **Kaam karne ka tareeqa** | Saare servers aik sath live traffic handle karte hain. | Aik server chal raha hota hai, dusra sirf backup ke liye baitha hota hai. |
| **Resource Usage** | 100% utilization (Paisa zaya nahi hota). | Backup server idle rehta hai jab tak failover na ho. |
| **Best For** | High availability & global scaling. | Disaster Recovery (DR) backups. |

---

14-October-2026

15-October-2026
