27-September-2026

---

We want the volume we are going to create from snapshot must be encypted.

snapshot may be ennrypted or unencypted. 



- `Enable the EBS Encryption By Default feature for the AWS Region.`

<img width="1506" height="1110" alt="EBS_Encryption_By_Default" src="https://github.com/user-attachments/assets/6a99dfcc-5920-4a6f-9429-51f2df412d65" />



> **Bottom Line:** Encrypted snapshot se banne waala volume *hamesha* Encrypted hota hai. Lekin unencrypted snapshot se banne waala volume sirf tabhi Encrypted banega jab **Region-level Default Encryption ON** ho!

---
---
---


<img width="1308" height="814" alt="auto-scaling-group-121523" src="https://github.com/user-attachments/assets/9636cfa7-f8ee-4400-8c8e-a0aa68ae9bd3" />

## Core Formula for SAA-C03 💡

> **Backlog Per Instance Metric:**
> $\text{Backlog Per Instance} = \frac{\text{Total Messages in SQS Queue}}{\text{Total Active EC2 Instances}}$
> Is formula se AWS ko pata chalta hai ke har EC2 instance ke hisse mein kitna kaam hai, aur us hisab se EC2 instances **Auto-Scale** hote hain.

---
---
---

