
<img width="1432" height="1135" alt="Amazon+EFS+-+Performance+and+Throughput+mode+-+SAA-C02 (2)" src="https://github.com/user-attachments/assets/7f4cbfbd-7761-4c5c-9887-3e53bbe67487" />


Chaliye isay bilkul asan alfaaz mein tod kar samajhte hain! 🧠✨

Is lambi line ka har hissa darasal yeh keh raha hai:

### 1. Amazon EFS (Elastic File System)

* Yeh ek **shared folder** ki tarah hai jise bohot saare ECS containers ek sath access kar sakte hain.

### 2. Max I/O Performance Mode

* Kyunke system mein bohot saare tasks aik sath chal rahe hain aur lagataar high-speed read/write ho raha hai, yeh mode **bohot saare operations** ko ek sath bina kisi rukawat ke handle karta hai.

### 3. Provisioned Throughput Mode

* Normal EFS mein speed data ke size par milti hai. Lekin yahan data sirf **10 TB** hai (jo ke speed ke liye thori kam hai). Isliye hum apni marzi ki **high speed (throughput) alag se fix/purchase** kar lete hain taake performance shandaar rahay.

### 4. Container mount point in ECS task definition

* Iska asan matlab yeh hai ke is shared folder ko ECS containers ke sath jod (mount) dena hai taake containers apna data foran ishi folder mein parh aur likh sakein.

---
---
---



<img width="1326" height="695" alt="aws-global-accelerator-endpoint-group (1)" src="https://github.com/user-attachments/assets/c9e8a21a-48be-4384-8bee-0b684fc8b890" />


Bilkul sahi pakra aap ne! Aap ka logic **100% theek** hai. 🎯

Isay mazeed wazeh kar lete hain:

### 1. ALB is Regional (Region-Specific) 🌍

* Har **Application Load Balancer (ALB)** aik specific region aur VPC ke andar banta hai.
* Agar aap ki application 3 mukhtalif regions (jaise US East, Europe, aur Asia) mein chal rahi hai, toh aap ke paas **3 alag ALBs** honge, aur har ALB ke apne alag public IP addresses honge jo waqt ke sath badal bhi sakte hain.

### 2. Isiliye Global Accelerator Chahiye! 🚀

* Jab corporate firewall par itne saare regions ke dynamic IPs whitelist karne parhte hain, toh admin ke liye yeh aik bara sir dard ban jata hai.
* **AWS Global Accelerator** kya karta hai? Yeh aap ko poori duniya ke liye sirf **2 fixed Static Anycast IP addresses** deta hai.
* Aap apnay corporate firewall par sirf yeh **2 static IPs ek dafa whitelist** kar dete hain. Jab koi user access karta hai, toh Global Accelerator khud peeche se traffic ko pakar kar us regional ALB tak safely pohchwa deta hai jahan application chal rahi hai.

---
---
---



<img width="453" height="165" alt="image11 (2)" src="https://github.com/user-attachments/assets/578d6bda-1182-484f-b410-aeab2b03ee90" />


### Sawal Kya Keh Raha Hai? ❓

* **Scenario:** Ek agency AWS par EC2 instance host kar rahi hai aur us par **Enhanced Networking** enable karne ko kaha gaya hai.
* **Requirement:** In options mein se kaun se **do (TWO)** sab se baray aur valid use cases hain jin ke liye Enhanced Networking use ki jati hai?

---

### Correct Answers (Select TWO) 💡

1. **When you need a consistently lower inter-instance latencies**
2. **When you need a higher packet per second (PPS) performance**

---

### Asan Alfaz Mein Wazahat (Explanation) 🔍

* **Enhanced Networking:** Yeh ek aisi feature hai jo **SR-IOV (Single Root I/O Virtualization)** technology use karti hai taake network performance ko maximize kiya ja sakay. Iske do main fawaid hain:
1. **Low Latency:** Do alag alag EC2 instances ke darmiyan data aane-jaane ka delay (**latency**) bohat kam aur stable ho jata hai.
2. **High Packet Per Second (PPS):** Yeh network per second bohat zyadah data packets ko handle karne ki salahiyat (**high throughput & PPS performance**) deta hai, jo ke high-performance computing ya data-heavy applications ke liye bohat zaroori hai.



---

### Baqi Options Ghalat Kyun Hain? ❌

* **High latency networking:** Latency kabhi bhi high nahi chahiye hoti; humein hamesha **low** latency chahiye hoti hai.
* **Low packet-per-second performance:** Enhanced Networking ka maqsad low PPS nahi balkay **high PPS** dena hota hai.
* **Dedicated connection to on-premises:** On-premises data center ke sath dedicated connection ke liye **AWS Direct Connect** ya VPN use hota hai, na ke Enhanced Networking.

---


25-October-2026



25-October-2026
