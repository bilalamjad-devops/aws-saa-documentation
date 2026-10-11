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



10-October-2026
