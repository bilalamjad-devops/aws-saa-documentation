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


29-October-2026

30-October-2026

31-October-2026

8-November-2026
