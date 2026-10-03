
Haan, bilkul! **AWS Instance Scheduler** asal mein ek pre-made CloudFormation template hota hai jise AWS khud provide karta hai.

Jab aap is template ko deploy karte hain, toh yeh aapke account mein EventBridge rules, Lambda functions, aur DynamoDB tables create kar deta hai. Is ke zariye aap easily tags (jaise `Schedule: office-hours`) ke thor par **EC2 aur RDS instances ko automatically fixed timing par ON aur OFF (start/stop)** kar sakte hain.

---

### Key Takeaway for AWS Exam:

* **Working Hours / Scheduled Stop-Start:** Jab bhi question mein EC2/RDS ko fixed business hours par stop/start karke cost save karne ka poocha jaye, toh **AWS Instance Scheduler** sab se best, automated aur minimal effort wala solution hota hai.






03-October-2026
