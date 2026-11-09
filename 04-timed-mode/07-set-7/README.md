6. Question

A company has migrated its containerized workloads into the AWS Cloud. The microservices applications are hosted on Amazon EC2 instances with Docker installed, Amazon Elastic Container Service (Amazon ECS), and newer deployments are hosted on Amazon Elastic Kubernetes Service (Amazon EKS). The company is using open-source tools such as Prometheus and Grafana installed on a virtual machine in its data center for monitoring its applications. The management wants to use the same tools for monitoring its containerized applications in its cloud environment.

Which of the following options is the recommended implementation for this scenario?

- `Create a workspace on AWS Manage Service for Prometheus to collect container metrics. Set this workspace as the data source in AWS Managed Grafana for monitoring and data visualization.`
- Export the on-premises VM and upload the image to an Amazon S3 bucket. Use VM Import/Export to import the image and launch it as an Amazon EC2 instance. Reconfigure Prometheus and Grafana to monitor the containerized cloud workloads.
- Create a new Amazon ECS cluster. Deploy the Prometheus and Grafana containers on this cluster for monitoring containerized workloads. Configure Prometheus for container metrics collection and Grafana for monitoring and data visualization.
- Create a new workspace AWS Managed Grafana to collect container metrics. Associate this as a data source in Amazon Managed Service for Prometheus. Configure data queries on Prometheus to visualize, monitor, and create alerts for the containerized workloads.

Koi baat nahi! Is question ko bilkul simple lafzon mein samajhte hain:

---

### 1. Problem Kya Thi?

* Company pehle se apne data center mein **Prometheus** aur **Grafana** (monitoring aur graphs dekhne ke tools) use kar rahi thi.
* Ab company AWS cloud par shift ho chuki hai aur woh chahti hai ke **wahi purane tools** use karein, lekin bina khud servers manage kiye (yani AWS unka khayal rakhe).

---

### 2. Sahi Jawab (AWS Managed Services) Kyun Behtar Hai?

AWS kehta hai: *"Aapko apni purani machine utha kar AWS par laane ki zaroorat nahi hai, aur na hi containers khud manage karne ki zaroorat hai. Hum aapko inhi tools ka **Managed version** dete hain:"*

1. **AWS Managed Service for Prometheus (AMP):** Yeh sara data/metrics chup chap collect karta rehta hai.
2. **Amazon Managed Grafana (AMG):** Yeh AMP se data uthata hai aur aapko khobsurat graphs aur dashboards ki shakal mein screen par dikhata hai.

---

### 3. Baaqi Options Kyun Galat Hain?

* **Purani VM ko AWS par lana:** Isme server ki maintenance, patches, aur security ki tension khud leni parti hai (bohot zyada mehnat).
* **ECS par khud Prometheus/Grafana ke containers chalana:** Isme bhi setup aur management ka overhead barh jata hai jabke AWS directly bani-banai managed service de raha ho.

---
---
---

9. Question

A startup company running applications on multiple Amazon EC2 instances is utilizing Amazon FSx for NetApp ONTAP for its shared storage on its primary region. File shares in the region are configured over NFS and CIFS protocols.

As part of resiliency requirements, the company needs to implement a storage disaster recovery (DR) solution in a secondary region using the same protocol as the primary region.

Which of the following satisfies the given requirements with the LEAST amount of management overhead?

- Launch an FSx instance in both source and destination regions, then establish connectivity with a VPC peering connection. Set up an AWS DataSync agent near your source data. Configure AWS DataSync service to replicate data from the source to the destination.
- Using on-demand data replication, set up an automated snapshot schedule of the Amazon FSx file system to an S3 bucket. Use the CopySnapshotAndUpdateVolume API to copy the snapshot to the destination region. Use the CreateVolume API to create a volume on the destination and mount the new volume to the destination file system.
- Provision an FSx instance in both source and destination regions, then establish connectivity with a VPC peering connection. Using AWS Backup, create a volume backup from the source region. Copy the backup volume to the destination region and restore using the Amazon FSx console.
- `Create an FSx for ONTAP instance in both source and destination regions, then establish connectivity with a VPC peering connection. Configure NetApp SnapMirror to replicate data between the two instances.`

<img width="565" height="467" alt="CreateAmazonFSxForNetAppONTAP (2)" src="https://github.com/user-attachments/assets/6f07e50e-4f7a-4673-8234-23c3ab11d6b0" />

<img width="1552" height="505" alt="AmazonFSxForNetAppONTAP (1)" src="https://github.com/user-attachments/assets/8451d299-6966-46f6-b0a6-dffb33bdd560" />



Aasan lafzon mein samajhte hain:

---

### 1. Simple Example (Real-World Analogy)

Is pooray sawal ka maqsad **Disaster Recovery (Backup/DR)** setup karna hai:

* Imagine karo aapki main file drive **Region A (Karachi)** mein chal rahi hai.
* Aap chahte ho ke sab files automatically **Region B (Lahore)** wale server par copy hoti rahein taake agar Karachi ka data center tabah bhi ho jaye, toh Lahore se kam foran shuru ho sakay.

---

### 2. NetApp ONTAP Kya Hai?

* **NetApp** ek mashhoor storage software company ka naam hai.
* **ONTAP** unka flagship **Storage Operating System / Engine** hai jo file systems, fast data storage, aur enterprise features (deduplication, fast snapshots, compression) handle karta hai.
* **AWS FSx for NetApp ONTAP:** AWS ne NetApp ke sath partnership karke unka pura ONTAP software system AWS cloud par fully-managed service ke tor par de diya hai.

---

### 3. SnapMirror Kya Hai Aur Kyun Use Hua?

* **SnapMirror** NetApp ONTAP ka apna **built-in (native) feature** hai.
* Iska kaam hi sirf yeh hai ke ek NetApp storage server se data ko doosre NetApp storage server par **automatically replicate (copy)** karta rahe.
* **Kyun Choose Kiya?** Kyunki yeh feature storage engine ke andar **pehle se built-in (pehle se bana hua)** hota hai. Humein koi alag se software/agent install nahi karna parta, na manual script likhni parti hai — bus ek click par replication shuru ho jati hai (minimal overhead).

---

### 💡 AWS Exam Tip (Rule of Thumb):

> Jab bhi exam mein **"FSx for NetApp ONTAP"** ka **Disaster Recovery (DR)** ya **Replication** pucha jaye, toh hamesha **NetApp SnapMirror** ko select karna hai!

---
---
---

13. Question

A company faces performance degradation due to intermittent traffic spikes in its application. The application is deployed across multiple EC2 instances within an Auto Scaling group and is fronted by a Network Load Balancer (NLB). The operations team found out that HTTP errors are not being detected by the NLB. As a result, clients are continuously routed to unhealthy targets and are never replaced, which impacts the availability of the application.

Which solution could resolve the issue with the least amount of development overhead?

- Configure CloudFront as a content delivery network (CDN) in front of the NLB to reduce the load on the instances and improve performance.
- `Configure the NLB to perform HTTP health checks on the critical paths of the application.`
- Use an Application Load Balancer (ALB) in place of the NLB. Enable HTTP health checks using the application's path.
- Increase the NLB's idle timeout to allow more time for requests to be processed, reducing the occurrence of intermittent HTTP/s errors.

<img width="1920" height="1003" alt="NLB-Health-Check-19-06-2023 (2)" src="https://github.com/user-attachments/assets/1d57f9b7-07db-4bba-a199-6de95eae51dd" />


No problem! Aayein isko ek simple real-world example se step-by-step samajhte hain:

---

### 1. Masla Kya Tha? (The Problem)

Maan lein aapki ek dukaan (application) hai aur aapne bahar ek guard (**NLB**) bithaya hua hai.

* Pehle guard sirf yeh dekhta tha ke **dukaan ka darwaza khula hai ya nahi** (TCP Port Check).
* Darwaza khula hota tha, toh guard customer ko andar bhej deta tha.
* Lekin andar **dukaan ka counter kharab ho chuka tha** (HTTP 500 Error). Customer andar jata tha, kharab service milti thi, aur wapas chala jata tha.
* Guard ko nahi pata chalta tha ke counter kharab hai, kyunki usne sirf darwaza khula dekha tha!

---

### 2. Solutions Ka Comparison

Aapke paas is masley ko hal karne ke do tarike hain:

| Tareeqa | Kya Karna Parega? | Mehnat (Overhead) |
| --- | --- | --- |
| **Option A (Guard badal kar naya Guard lana - ALB)** | Pura NLB hatao, naya ALB lagao, DNS badlo, security groups naye sir se set karo. | **Boht Zyada Mehnat** (High Overhead) |
| **Option B (Purane Guard ko Training do - NLB HTTP Check)** | Guard (NLB) wahi rahega, bas usko bolo: *"Ab se sirf darwaza mat dekhna, andar ja kar counter (HTTP Path) bhi check karna ke service mil rahi hai ya nahi."* | **Boht Kam Mehnat** (Least Overhead) |

---

### 🔑 Core Takeaway (Exam Rule)

AWS NLB ke paas yeh feature hota hai ke woh **HTTP Health Check** kar sakta hai.

Jab hum bas NLB ki setting mein ja kar Health Check ko `TCP` se badal kar `HTTP` (path e.g. `/health`) kar dete hain, toh:

1. NLB HTTP errors ko detect karna shuru kar deta hai.
2. Unhealthy server par traffic bhejna band kar deta hai.
3. Aur is kaam ke liye humein **poora Load Balancer replace nahi karna parta**, jis se hamara time aur mehnat dono bachti hain (**Least Development Overhead**).

---
---
---


21. Question

A Solutions Architect must ensure that AWS resources in an Amazon VPC do not exceed their service limits. To achieve this, the architect requires a system that provides recommendations to ensure resource usage aligns with AWS best practices.

Which service is the MOST effective for this task?

- AWS Cost Explorer
- `AWS Trusted Advisor`
- Amazon Inspector
- AWS Budgets

<img width="1844" height="787" alt="TD_AWS_Trusted_Advisor_14Nov2024 (1)" src="https://github.com/user-attachments/assets/31477f5f-051c-43ad-94fa-69f8a7f3e1df" />


Aayein isko ek simple example se samajhte hain:

---

### 1. Masla Kya Hai? (The Problem)

Har AWS account mein har resource ki ek **limit (quota)** hoti hai (jaise ek VPC mein kitne Elastic IPs ho sakte hain ya kitne EC2 instances chal sakte hain).

Agar aapki limit poori ho jaye aur aap naya resource banane ki koshish karein, toh AWS error de deta hai.

---

### 2. Solution: AWS Trusted Advisor

**AWS Trusted Advisor** aap ke account ke **Personal Doctor ya Inspector** ki tarah hai.

Yeh hamesha background mein chal raha hota hai aur AWS ki **Best Practices** ke mutabiq aapko alert karta hai. Is ke paas ek khas **Service Limits Check** hota hai jo aapko pehle hi bata deta hai:

> *"Suno! Aapki Elastic IP limit 80% tak poori ho chuki hai, is se pehle ke aapka kaam ruke, limit barhane ki request bhej do."*

---

### 3. Baaqi Options Kyun Galat Hain?

* **AWS Cost Explorer / AWS Budgets:** Yeh dono sirf **paise (cost/bills)** ki monitoring karte hain. Inka resource ki technial limit se koi taalluq nahi.
* **Amazon Inspector:** Yeh sirf **security vulnerabilities (hacking aur viruses)** scan karta hai, service limits check nahi karta.

---

### 🔑 Exam Tip:

> Jab bhi exam sawal mein **"Service Limits"** aur **"AWS Best Practices"** ke lafz ek sath aayein, toh 99% cases mein jawab **AWS Trusted Advisor** hota hai.

---
---
---

### 🧠 Question Kya Keh Raha Hai? (Scenario Breakdown)

1. **Setup:** Ek VPC ke andar AWS resources hain aur Solutions Architect yeh ensure karna chahta hai ke resources apne **service limits (quotas)** ko cross na karein.
2. **Requirement:** Ek aisa system chahiye jo **AWS best practices** ke mutabiq recommendations de aur service limits ki monitoring kare.

---

### 🔍 Keyword Scanning (Sawal se Sahi Jawab Tak)

* **Keywords in Question:** `"service limits"`, `"recommendations"`, `"AWS best practices"`.
* **Matching AWS Feature:**
* **AWS Trusted Advisor:** Yeh ek aisi managed service hai jo aapke poore AWS environment ko inspect karti hai aur **5 categories** (Cost Optimization, Security, Fault Tolerance, Performance, aur **Service Limits**) mein best practice recommendations deti hai. Service limits/quotas check karne ke liye Trusted Advisor sab se best tool hai.



---

### ❌ Baaqi Options Kyun Galat Hain?

* **AWS Cost Explorer:** Yeh sirf costs aur spending trends ko analyze karne ke liye hota hai, service limits ya best practices recommendations ke liye nahi.
* **Amazon Inspector:** Yeh EC2 instances, containers, aur Lambda functions mein security vulnerabilities aur network reachability scan karta hai.
* **AWS Budgets:** Yeh cost aur usage limits set karne aur alert generate karne ke liye hota hai jab aapka budget exceed hone lagta hai, resource service limits (jaise VPC limits ya EC2 limits) ke liye nahi.

---

### ✅ Correct Answer:

* **AWS Trusted Advisor**

---
---
---


22. Question

An organization seeks to enhance its customer experience by integrating and visualizing its diverse data sources. The organization has a robust Amazon S3 data lake, governed by AWS Lake Formation, containing detailed purchase histories. Additionally, they store operational data, including real-time inventory and sales information, in an Amazon Aurora MySQL database.

The goal is to create a dynamic visualization in Amazon QuickSight, joining the data lake content with the Aurora database. The company requires stringent column-level authorization, allowing its marketing team to access only specific columns in the database for targeted promotions and customer insights.

What is the most efficient solution to meet these requirements with minimal operational overhead?

- Leverage AWS Glue Elastic Views to create a materialized view of the Aurora database in Amazon S3. Implement an S3 bucket policy for column-level access control for QuickSight users, and use Amazon S3 as the data source in QuickSight.
- Utilize AWS Glue Studio to transfer data from the Aurora database to the S3 data lake. Apply an IAM policy to QuickSight users for column-level access control, and then use Amazon S3 as the data source in QuickSight.
- `Employ a Lake Formation blueprint to incrementally ingest data from the Aurora database to the S3 data lake, then enforce column-level access control by creating a Lake Formation data filter with the specified columns. Set up Amazon QuickSight to use Amazon Athena as its data source.`
- Configure an Amazon EMR job to ingest data directly from the Aurora database to QuickSight's SPICE engine, filtering to include only the necessary columns.

<img width="1268" height="1010" alt="ui_aws_lake_formation_columnar_level_access_control (2)" src="https://github.com/user-attachments/assets/b5811aa2-7ec8-413a-843d-9be2c020b8b0" />

<img width="1552" height="648" alt="aws_lake_formation_columnar_level_access_control (2)" src="https://github.com/user-attachments/assets/394da3ff-7e08-4cce-8770-5a27854c6947" />


Koi baat nahi! Aayein isay bhi bohot aasan daily-life example se samajhte hain:

---

### 1. Masla Kya Hai? (The Problem)

Company ke paas do alag-alag jagah par data pada hai:

1. **Amazon S3** par purana customer data.
2. **Aurora Database** par inventory ka data.

Marketing team ko inn dono data ko milakar **QuickSight** mein graphs dekhne hain.

Lekin masla yeh hai ke Marketing team ko **saare columns nahi dikhane** (jaise secret prices ya personal details wale columns chupane hain). Isay kehte hain **Column-Level Access Control**.

---

### 2. Sahi Jawab (AWS Lake Formation) Kyun Hai?

AWS ne data lakes ko secure karne ke liye ek special service banayi hai jiska naam hai **AWS Lake Formation**. Iska kaam hi data lake ki **security aur governance** sambhalna hai.

* **Blueprints:** Aurora se automatic data S3 data lake mein lane ke liye kisi coding ki zaroorat nahi parti, Lake Formation ka **Blueprint** feature yeh kaam direct kar deta hai.
* **Data Filters:** Is feature ke zariye aap easily tick kar sakte hain ke: *"Marketing team sirf Column A aur Column B dekh sakti hai, baaki hide kar do."*
* **Athena + QuickSight:** QuickSight data ko Athena ke zariye read karta hai, aur Athena auto-check karta hai ke Lake Formation ne kya permissions di hain.

---

### 3. Baaqi Options Kyun Galat Hain?

* **S3 Bucket Policy (Option 1):** S3 bucket policy yeh toh bata sakti hai ke kis file ko download karna hai, lekin file ke **andar ke columns ko hide nahi kar sakti**.
* **IAM Policy (Option 2):** IAM policy bhi AWS services ki permissions deti hai, yeh kisi database file ke **andar individual columns nahi chupa sakti**.
* **Amazon EMR (Option 4):** EMR chalana bohot mushkil aur costly kaam hai, jabke Lake Formation bina kisi mehnat ke easily ho jata hai.

---

### 🔑 Exam Tip:

> Jab bhi exam mein **"S3 Data Lake"**, **"QuickSight / Athena"**, aur **"Column-level / Row-level security"** ki baat ho, toh hamesha **AWS Lake Formation (Data Filters)** hi sahi jawab hota hai!

---
---
---

26. Question

A company is planning to deploy a Secure File Transfer Protocol (SFTP) service on AWS. The SFTP service must receive traffic only from approved public IP addresses. The company should be able to add users and manage their permissions on the SFTP server. The company seeks a highly available serverless solution that can deliver high IOPS performance.

Which of the following solutions would fulfill the company’s needs?

- Create an Amazon S3 bucket. Set up an SFTP service with a public endpoint in AWS Transfer Family filtered exclusively for trusted IPs. Link the S3 bucket to the service's endpoint, then manage user permissions.
- Set up a server-based SFTP solution using Amazon EC2 instances in a public subnet within a VPC. Secure the setup with security groups that restrict access to approved IP addresses. Attach an encrypted Amazon S3 bucket for storage and configure user access permissions through IAM roles.
- `Launch an encrypted Amazon EFS volume. Set up an AWS Transfer Family for SFTP service utilizing elastic IP addresses with a publicly accessible VPC endpoint. On the endpoint, apply a security group restricting access to trusted IPs. Integrate the EFS volume with the service's endpoint and enable user access.`
- Launch an encrypted Amazon Elastic Block Store (EBS) volume. Set up an SFTP service with a public endpoint in AWS Transfer Family and configure it to allow connections only from verified IP sources. Associate the EBS volume with the SFTP service's endpoint and grant users access.


<img width="1920" height="977" alt="setting-up-SFTP-04-13-2024 (1)" src="https://github.com/user-attachments/assets/ce3f5fe0-289b-4ec9-9d94-80d18449ce40" />

Aayein isay step-by-step bohot simple lafzon mein samajhte hain:

---

### 1. Requirements (Sawal Ki Shartein)

1. **SFTP Service:** Files upload/download karne ke liye SFTP chahiye.
2. **IP Whitelisting:** Sirf specific trusted IPs hi connect kar sakein.
3. **Serverless:** Koi EC2 instance ya server khud manage na karna pare.
4. **Storage:** High performance/IOPS mil sake.

---

### 2. Sahi Option Kyun Sahi Hai?

* **AWS Transfer Family:** Yeh AWS ki fully-managed (serverless) service hai jo SFTP support karti hai. Is se humein koi EC2 server nahi banana parta.
* **VPC Endpoint + Security Group:** trusted IPs ki restriction lagane ke liye Transfer Family ko **VPC Endpoint** ke sath lagaya jata hai, jahan **Security Group** ke zariye hum specific IPs ko allow karte hain (Public endpoint par Security Group nahi lagta).
* **Amazon EFS:** EBS storage ko Transfer Family ke sath direct attach nahi kiya ja sakta. AWS Transfer Family sirf **S3 aur EFS** ko support karta hai. EFS high performance aur file-system structure de deta hai.

---

### 3. Short Summary Table

| Requirement | Solution | Kyun? |
| --- | --- | --- |
| **Serverless SFTP** | **AWS Transfer Family** | Fully managed service hai, no EC2 management. |
| **IP Restrict Karna** | **VPC Endpoint + Security Group** | Trusted IPs ko allow karne ke liye Security Group use hota hai. |
| **Supported Storage** | **Amazon EFS** | EBS support nahi hota, aur EFS high performance deta hai. |

---
---
---

28. Question

A serverless application has been launched on the DevOps team’s AWS account. Users from the development team’s account must be granted permission to invoke the Lambda function that runs the application. The solution must use the principle of least privilege access.

Which solution will fulfill these criteria?


- On the function’s execution role, add a permission that includes the lambda:InvokeFunction as action and arn:aws:iam::[DEV AWS Account Number]:root as principal.
- On the function’s execution role, add a permission that includes the lambda:* as action and arn:aws:iam::[DevOps AWS Account Number]:root as principal.
- On the function’s resource-based policy, add a permission that includes the lambda:* as action and arn:aws:iam::[DevOps AWS Account Number]:root as principal.
- `On the function’s resource-based policy, add a permission that includes the lambda:InvokeFunction as action and arn:aws:iam::[DEV AWSAccount Number]:root as principal.`

<img width="1152" height="674" alt="aws-lambda-iam-policy-secure-access" src="https://github.com/user-attachments/assets/2f83339f-162a-40c3-b567-b0d8456e883d" />



### 🎯 What is the question saying?

There are **2 AWS accounts**:

- **DevOps account** → contains the Lambda function
- **Development account** → developers need permission to **invoke** that Lambda

The requirement is:

> Developers from another AWS account should be able to call the Lambda, with **least privilege**.

So we need to answer **two things**:

1. **Where do we give the permission?**
2. **What exact permission do we give?**

---

### ✅ Correct answer

**Option 4:**

> On the Lambda function’s **resource-based policy**, allow `lambda:InvokeFunction` for `arn:aws:iam::[DEV AWS Account Number]:root`.

### 🔑 Why?

Lambda has a **resource-based policy** specifically for saying:

> "Who is allowed to invoke this Lambda?"

For cross-account access, the Lambda resource policy can trust another AWS account.

Conceptually:

```text
Development Account
        │
        │ lambda:InvokeFunction
        ▼
   Lambda Function
   (DevOps Account)
        │
        └── Resource-based policy
              │
              └── Allows Dev Account
```

And **least privilege** means:

```text
lambda:InvokeFunction
```

not:

```text
lambda:*
```

Because developers only need to **invoke** the function, not perform every Lambda action.

---

### 🧠 What is a resource-based policy?

Think of it like this:

**Identity-based policy** = permission attached to the **person/role**

> "What can this user do?"

**Resource-based policy** = permission attached to the **AWS resource**

> "Who can access this resource?"

For Lambda cross-account access, the Lambda's **resource-based policy** is the important part.

---

### ❌ Why the other options are wrong?

**Option 1 — Execution role ❌**

The Lambda **execution role** answers:

> "What can the Lambda do?"

Example:

```text
Lambda → S3
Lambda → DynamoDB
Lambda → CloudWatch
```

It does **not** grant other accounts permission to invoke the Lambda.

---

**Option 2 — Execution role + `lambda:*` ❌**

Wrong policy type **and** too much permission.

---

**Option 3 — Resource policy + `lambda:*` ❌**

Correct policy type, but violates **least privilege**.

Developers only need:

```text
lambda:InvokeFunction
```

not every Lambda action.

---

### 🔥 Exam shortcut

Remember this:

> **Who can invoke my Lambda? → Lambda resource-based policy**

> **What can my Lambda access? → Lambda execution role**

And:

> **Least privilege → exact action, not `service:*`**

So:

**Cross-account Lambda invocation → Resource-based policy + `lambda:InvokeFunction`** ✅

---
---
---


33. Question

A company has a mission-critical, public-facing containerized application running on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster that has a microservices architecture.

There is a requirement to implement a fully managed centralized application logging solution that automatically collects metrics and application logs. The company also requires a dashboard to easily monitor the application metrics.

Which of the following can meet the above requirements with the LEAST amount of operational overhead?

- Use Amazon OpenSearch Service with an integration to an Amazon EKS cluster to collect and store application logs and metrics. Use OpenSearch Dashboards to monitor and visualize the application’s performance.
- `Set up and configure Amazon Managed Service for Prometheus metrics collection for the EKS cluster by installing the Prometheus agent on all EKS pods. Launch a dashboard in Amazon Managed Grafana to visualize metrics and logs in Amazon EKS.`
- Use Amazon CloudWatch Container Insights with the Amazon EKS cluster. Use the CloudWatch dashboard to view the application logs and metrics.
- Install and set up the Amazon CloudWatch agent on the Amazon EKS cluster to collect the metrics. Use the CloudWatch dashboard to view the application logs.

<img width="1352" height="983" alt="container_insights_aws (1)" src="https://github.com/user-attachments/assets/d099de5b-1a92-40a9-ad57-58af230dfeab" />


Bhai, aapne bilkul theek point uthaya hai! TutorialDojo (jo SAA-C03 ki tayari ke liye best platform hai) ke kuch sawaalon mein Prometheus aur Grafana wala option bhi correct diya jata hai.

Lekin is sawal mein ek chota sa **catch (farq)** hai jo option ko galat ya sahi banata hai. Aaiye isay Roman Urdu mein detail se samjhte hain:

---

### 🔍 TutorialDojo ke us Sawal aur is Sawal ka Asal Farq

Agar aap TutorialDojo wale option ko ghaur se padhein, toh wahan do baatein hain:

1. **Metrics** ke liye Prometheus aur Grafana use hote hain (yeh bilkul theek hai, Prometheus EKS metrics ke liye industry standard hai).
2. Lekin **Logs** ko dekhne ke liye Prometheus ya Grafana direct use nahi hote! Grafana logs dikha sakta hai agar uske sath Loki ya OpenSearch jaisa tool attached ho, lekin standard Prometheus/Grafana setup **metrics** ke liye hota hai, logs ke liye nahi.

Asal mein **Amazon CloudWatch Container Insights** ek aisi aisi built-in managed feature hai jo **aik hi click par metrics aur logs dono** baghair kisi extra tool ke provide kar deti hai, is liye isme **Least Operational Overhead** hai.

---

### ⚖️ Kab Prometheus/Grafana aur kab Container Insights?

| Scenario | Best Choice | Kyun? |
| --- | --- | --- |
| **Agar sirf Kubernetes/EKS Metrics chahiyein** (aur aap open-source Prometheus/Grafana standard use karna chahte hain) | **Amazon Managed Service for Prometheus (AMP) + Amazon Managed Grafana (AMG)** | Yeh Kubernetes community ka favorite tool hai. |
| **Agar least operational overhead ke sath Metrics + Logs dono ek sath chahiyein** bina extra agents ke | **Amazon CloudWatch Container Insights** | AWS ka native managed solution hai, koi extra software manage nahi karna parta. |

TutorialDojo ka maqsad aapko alag-alag architectures sikhana hota hai, lekin exam ki requirement (**Least Operational Overhead** + **Metrics + Logs dono**) ko dekhte hue CloudWatch Container Insights sab se aasan aur fast choice banta hai!

---

29-October-2026

30-October-2026

31-October-2026

8-November-2026
