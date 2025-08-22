---
title : "Setting up security automation"
date : 2025-08-10
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

We need to set up a few more components for our security automation pipeline

### 1. Create Athena Table
The `TimelineAnalysisLambda` needs a table in Athena to query your CloudTrail logs. You must create this manually.
* Go to the **Amazon Athena** console.
* Follow the steps in the **[AWS Documentation](https://docs.aws.amazon.com/athena/latest/ug/create-cloudtrail-table-ct.html)** to create a table from the logs in your new `CloudTrailBucket`. The bucket's name is available in the **Outputs** tab of your CloudFormation stack.
* **Note:** Choose ```<Stack Name>-cloudtrailbucket-abcdefgh``` when asked where to set up your **Athena Table** 

![Athena Table](/images/00/0019.png?featherlight=false&width=90pc)

### 2. Edit Timeline Analysis
Once you have created the Athena Table, note the resulting table name and go to `TimelineAnalysisLambda`
* Go to the **Amazon Lambda** console.
* Go to `TimelineAnalysisLambda`.
* Click on **Configuration** and **Environment Variables**
* Click on **Edit** and add a new variable named `ATHENA_TABLE_NAME`
* Fill in the table name of the **Athena** table you created for step 1

![Timeline Analysis](/images/00/0020.png?featherlight=false&width=90pc)


### 3. SNS Subscription
Head over to the SNS service for Amazon and click on the Topic that is associated with the CloudFormation stack.
Click on "Create Subscription" and enter your email. Confirm the subscription once the confirmation arrive at your email to receive incident reports

![SNS](/images/00/0021.png?featherlight=false&width=90pc)

---
### Ready for Testing
Now, everything is set and ready. You can move on to the next section to test our security pipeline