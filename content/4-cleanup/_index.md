---
title: "Project Cleanup Guide"
date: 2025-08-22
weight: 5
chapter: false
pre: " <b> 4. </b> "
---

This guide provides the steps to safely and completely remove all AWS resources created for this project. Following these instructions will ensure that you do not incur any ongoing costs. The primary method for cleanup is deleting the CloudFormation stack, which manages the entire lifecycle of our infrastructure.

**Content:**
- [Step 1: Empty the S3 Buckets (Prerequisite)](#step-1-empty-the-s3-buckets-prerequisite)
- [Step 2: Delete the Main CloudFormation Stack](#step-2-delete-the-main-cloudformation-stack)
- [Step 3: Clean Up Remaining Resources](#step-3-clean-up-remaining-resources)

---
### Step 1: Empty the S3 Buckets (Prerequisite) 🗑️

Before you can delete the CloudFormation stack, you **must** manually empty the S3 buckets it created. CloudFormation cannot delete buckets that contain files.

1.  **Find Your Bucket Names:** Navigate to the **CloudFormation** console, select your stack (e.g., `SecurityAutomationStack`), and go to the **Outputs** tab. Note the names of the `EvidenceBucketName`, `CloudTrailBucketName`, and `AthenaResultsBucketName`.

2.  **Navigate to S3:** Go to the **Amazon S3** console.

3.  **Empty Each Bucket:** For each of the three buckets identified in step 1, perform the following actions:
    * Click on the bucket name.
    * Select all the files and folders inside.
    * Click the **Delete** button.
    * Type `permanently delete` into the confirmation box and click **Delete objects**.

---
### Step 2: Delete the Main CloudFormation Stack 🚀

This single action will automatically remove the vast majority of your project's resources in the correct order.

1.  **Navigate to CloudFormation:** Go back to the **CloudFormation** console.
2.  **Select the Stack:** Make sure your project's stack is selected.
3.  **Delete the Stack:** Click the **Delete** button. You will be asked to confirm the deletion.

CloudFormation will now begin the deletion process, which can take several minutes. It will remove the Lambda functions, Step Function, API Gateway, IAM roles, and most other resources.

---
### Step 3: Clean Up Remaining Resources 🧹

A few resources will not be deleted by the stack, either by design or because they were created manually. You must remove these yourself to complete the cleanup.

* **S3 Buckets:** Your `EvidenceBucket` and `CloudTrailBucket` were configured with a `DeletionPolicy: Retain` as a safety measure.
    * **Action:** Go back to the **S3** console. Now that the buckets are empty and the stack is deleted, you can select these two buckets and **delete** them.

* **Athena Table:** The Athena table used to query CloudTrail logs was created manually, so CloudFormation does not manage it.
    * **Action:** Go to the **Amazon Athena** console. In the Query Editor, run the following command, replacing `your_cloudtrail_table_name` with the name you used:
      ```sql
      DROP TABLE your_cloudtrail_table_name;
      ```

* **CloudWatch Log Groups (Optional):** The log groups for your Lambda functions are not deleted by default. They don't typically incur costs and will expire based on their retention settings.
    * **Action:** For a perfectly clean account, you can go to the **CloudWatch** console, select **Log groups**, and manually delete the log groups associated with your project's Lambda functions.

Once these steps are complete, all resources for your project will have been successfully removed.