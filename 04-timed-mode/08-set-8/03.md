3. Question

The CTO of an e-commerce company wants to implement a continuous integration (CI) pipeline using AWS CodePipeline. The CTO wants to design the pipeline so that it automatically triggers whenever developers push code to the company’s private GitHub repository. The CTO also wants to ensure that the integration between AWS and GitHub is secure and auditable out of the box.

What is the easiest way to trigger CodePipeline automatically upon a code commit while satisfying security and auditability requirements?

- Set up Amazon API Gateway to expose a webhook endpoint that GitHub can call. Configure API Gateway to trigger CodePipeline via AWS Lambda. In the Lambda source code, invoke Amazon CloudWatch Logs APIs to perform logging.
- `Configure AWS CodeConnections to integrate a private GitHub repository with CodePipeline. Use AWS CloudTrail logs for monitoring and auditing.`
- Use Amazon EventBridge to poll the GitHub repository periodically and trigger the pipeline. Use AWS CloudTrail logs for monitoring and auditing.
- Deploy a GitHub webhook listener application written in Python on AWS Elastic Beanstalk, and configure it to call CodePipeline. Ensure that the application writes logs to Amazon CloudWatch Logs.

<img width="1680" height="876" alt="aws-codeconnections-2025Apr28" src="https://github.com/user-attachments/assets/bb3431e5-3428-4e02-a5ea-e870f9cd835c" />

Yeh raha aapka agla behtareen MCQ! Chaliye is question ko aasan lafzon mein samajhte hain ke yeh kya pooch raha hai aur iska sahi jawab kya hai.

---

### 🧠 Question kya keh raha hai? (Scenario Breakdown)

1. **Goal:** Ek e-commerce company ke CTO ko **AWS CodePipeline** ke zariye ek CI pipeline banani hai jo developers ke private **GitHub repository** par code push karte hi **automatically trigger** ho jaye.
2. **Key Requirements:**
* Automatic trigger upon code commit.
* Integration AWS aur GitHub ke darmiyan **secure aur auditable** honi chahiye (out of the box).



---

### 🔍 Options ka Tajziya (Analysis):

* **Option 1 (API Gateway + Lambda):** Iske zariye webhook banana aur Lambda likhna bohot complex aur "reinventing the wheel" hai. Jab AWS mein direct native integration mojood ho toh custom code likhne ki zaroorat nahi parti.
* **Option 3 (EventBridge polling):** EventBridge polling ke zariye periodic check karega, jo real-time commit trigger ke liye theek nahi hai aur isme extra configuration lagti hai.
* **Option 4 (Elastic Beanstalk Python app):** Apni khud ki webhook listener application deploy karna aur usay manage karna overhead hai, jabke security aur auditability ke liye native managed service available ho.
* **Option 2 (AWS CodeConnections + CloudTrail):** **AWS CodeConnections** (jo pehle *AWS CodeStar connections* kehlata tha) GitHub aur AWS services ke darmiyan secure, native, aur out-of-the-box connection provide karta hai jo code commits par pipeline ko automatically trigger karta hai. Sath hi, monitoring aur auditing ke liye **AWS CloudTrail** ka use hota hai jo security requirements ko poora karta hai.

---

### ✅ Correct Answer:

* **Configure AWS CodeConnections to integrate a private GitHub repository with CodePipeline. Use AWS CloudTrail logs for monitoring and auditing.**

---

Kya aapko yeh concept aur iska reason achi tarah samajh aa gaya? Agar agla MCQ hai toh aap share kar sakte hain!

4-November-2026

6-November-2026
