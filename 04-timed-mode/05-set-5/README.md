
* **AWS DataSync:** Kyunke on-premises storage khatam ho rahi thi aur millions of obsolete records ko *foran* AWS par shift karna tha, iske liye **AWS DataSync** sab se fast aur secure online data transfer/migration service hai. (Storage Gateway tab use hota hai jab data on-prem rakhte hue hybrid access chahiye ho, lekin yahan data ko poori tarah move karna tha).




* **CloudTrail Data Events:** Sawal mein pucha gaya hai ke **"each data access must be audited at all levels"**. CloudTrail ke andar **Management Events** sirf bucket creation ya settings change (control plane) ko track karte hain, jabki individual objects par hone wali data access (jaise `GetObject` ya `PutObject`) ko track karne ke liye **Data Events** enable karne parte hain.


23-October-2026
