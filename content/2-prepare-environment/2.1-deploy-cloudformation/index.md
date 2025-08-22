---
title : "Deploy using CloudFormation"
date : 2025-08-10
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

This guide provides step-by-step instructions for deploying the complete Security Automation platform using the AWS Management Console and a CloudFormation template.

### Prerequisites

Before you begin, you will need:
* An **AWS Account** with administrative permissions.
* The final **`IaC.yaml`** CloudFormation template file for this project, downloaded to your local machine.

---
### Step 1: Navigate to CloudFormation

1.  In the AWS Management Console, type "CloudFormation" into the search bar and select it.
2.  Ensure you are in your desired AWS Region.

---
### Step 2: Create the Stack

1.  From the CloudFormation dashboard, click the **Create stack** button.
2.  Select **With new resources (standard)**.

![CloudFormation Create Stack](/images/00/0002.png?featherlight=false&width=90pc)

---
### Step 3: Specify the Template

1.  Download the Infrastructure file from https://github.com/Finnard2112/Internship/blob/intern/Nguyen-Ha-Phan/project/IaC.yaml
2.  Under **Prepare template**, select **Template is ready**.
3.  Under **Template source**, select **Upload a template file**.  
3.  Click **Choose file** and select the downloaded `IaC.yaml` file from your local machine.
4.  Click **Next**.

![CloudFormation Choose Template](/images/00/0003.png?featherlight=false&width=90pc)

---
### Step 5: Specify Stack Details

1.  **Stack name:** Enter a unique name for your deployment (e.g., `SecurityAutomationStack`).
2.  There are no parameters to fill in for this template. Click **Next**.

---
### Step 6: Configure Stack Options

You can leave all the options on this page as their defaults for this project.

1.  Scroll to the bottom and click **Next**.

---
### Step 7: Review and Create

This is the final review page.

1.  Scroll to the very bottom to the **Capabilities** section.
2.  You **must** check the box that says: **"I acknowledge that AWS CloudFormation might create IAM resources with custom names."** This is required because the template creates several IAM Roles and Policies.
3.  Click **Create stack**.

![Finishing Stack](/images/00/0004.png?featherlight=false&width=90pc)

The deployment process will now begin and may take 5-10 minutes. You can monitor its progress in the **Events** tab. The status will show `CREATE_COMPLETE` when finished.
