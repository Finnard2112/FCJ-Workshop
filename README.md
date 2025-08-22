# 🛡️ Workshop: Building an Automated Security Incident Response Platform on AWS

> 📘 **Project documentation live at:**  
> 🔗 [https://github.com/Finnard2112/AWS_Security_Automation_Project](https://github.com/Finnard2112/AWS_Security_Automation_Project)

This is a **Hugo-based documentation site** for the project **Build a Serverless Security Automation Platform with AWS**. It's designed for Cloud Engineering, DevOps, and Cybersecurity students and professionals who want to learn how to build modern, automated, cloud-native security response systems.

> ✅ This workshop is part of the **AWS First Cloud Journey Internship Program (FCJ 2025)**  
> 📚 Language: English

---

## 📌 Project Overview

In this hands-on project, you will learn how to:

* 🚀 **Deploy a Serverless Application** using **API Gateway**, **Lambda**, and **DynamoDB**.
* 🛡️ **Detect Threats** automatically using **AWS GuardDuty**.
* 🔁 **Orchestrate Automated Responses** using **AWS Step Functions** and **EventBridge**.
* 💥 **Contain Threats** in near real-time using **AWS Lambda** and **IAM**.
* 🔍 **Perform Automated Forensic Analysis** with **Amazon Athena** and **AWS CloudTrail**.
* 📨 **Generate and Send Incident Reports** with **AWS Lambda** and **Amazon SNS**.

You will build a complete security response pipeline that automatically detects, contains, investigates, and reports on a simulated security threat.

---

## 📂 Repository Structure

content/
├── 1-introduction/
├── 2-prepare-environment/
│   ├── 2.1-deploy-cloudformation.md
│   ├── 2.2-configuring-application.md
│   └── 2.3-test-application.md
├── 3-testing-the-pipeline/
│   ├── 3.1-configure-event-bridge.md
│   ├── 3.2-prepare-test-event.md
│   ├── 3.3-setting-up-automation.md
    └── 3.3-testing-security-pipeline.md
├── 4-cleanup/
│   └── 4.1-cleanup.md
static/
└── images/


---

## 🚀 How to Run the Documentation Site

To run this Hugo-based documentation site locally:

```bash
git clone [https://github.com/Finnard2112/AWS_Security_Automation_Project.git](https://github.com/Finnard2112/AWS_Security_Automation_Project.git)
cd AWS_Security_Automation_Project
hugo serve

Open your browser at http://localhost:1313

🌐 Live Site (GitHub Pages)
📌 Hosted at:
https://finnard2112.github.io/AWS_Security_Automation_Project

📄 License
This project is for educational and portfolio use only.

🙇‍♂️ Credits

    Developed by: Nguyen Ha Phan

    Email: finnard2112@gmail.com