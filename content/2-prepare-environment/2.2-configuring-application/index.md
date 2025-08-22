---
title : "Configuring the Serverless Aplication"
date : 2025-08-10
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

After your CloudFormation stack has been successfully created, there are a few essential configuration steps you must complete manually to make the platform fully operational.

### 1. Adding Code to Your Lambda Functions
The CloudFormation template deployed all seven of your Lambda functions with simple placeholder code. You now need to upload the actual application and security logic for each one.

1.  **Prepare Your Code:** For each of your seven Lambda functions, visit the following link to download the lambda code and see additional instructions for configuration https://github.com/Finnard2112/Internship/tree/intern/Nguyen-Ha-Phan/project/lambdas

2.  **Navigate to the Lambda Console:** In the AWS Management Console, go to the **Lambda** service.

3.  **Find Your Functions:** You will see a list of the functions created by your stack, such as:
    * `SecurityAutomationStack-ProductService`
    * `SecurityAutomationStack-ScanService`
    * `SecurityAutomationStack-RemoveProduct`
    * `SecurityAutomationStack-ContainmentLambda`
    * `SecurityAutomationStack-EvidenceCollectionLambda`
    * `SecurityAutomationStack-TimelineAnalysisLambda`
    * `SecurityAutomationStack-GenerateReportLambda`

![Lambdas](/images/00/0005.png?featherlight=false&width=90pc)

4.  **Upload the Code:** For each of the seven functions, perform the following steps:
    * Click on the function's name.
    * In the **Code source** section, copy and paste the previously downloaded code into this section.
    * Change the file name to lambda_function.py
    * Click **Deploy**.
    
![Deploying Code](/images/00/0006.png?featherlight=false&width=90pc)

5.  **Repeat** this upload process for all seven functions, ensuring you upload the correct code package to each one.
---

### 2. Create a Test User in Cognito 
To test your application's API endpoints (like adding or removing a product), you need to authenticate as a real user. This step walks you through creating a test user in your new Cognito User Pool using the AWS CLI.

For more information on AWS CLI, visit this link: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html

1.  **Get Your User Pool and Client IDs:**
    * Open your terminal. You need to get two values from your CloudFormation stack's outputs. Replace `YourStackName` with the name you gave your stack (e.g., `SecurityAutomationStack`).
    ```bash
    # Get the User Pool ID
    USER_POOL_ID=$(aws cloudformation describe-stacks --stack-name YourStackName --query "Stacks[0].Outputs[?OutputKey=='UserPoolId'].OutputValue" --output text)

    # Get the User Pool Client ID
    USER_CLIENT_ID=$(aws cloudformation describe-stacks --stack-name YourStackName --query "Stacks[0].Outputs[?OutputKey=='UserPoolClientId'].OutputValue" --output text)
    ```
![User Pool and Client ID](/images/00/0007.png?featherlight=false&width=90pc)

2.  **Sign Up a New User:**
    * Run the following command, replacing `<test-email@example.com>` and `<YourStrongPassword!123>` with your desired credentials.
    ```bash
    aws cognito-idp sign-up \
      --client-id $USER_CLIENT_ID \
      --username <test-email@example.com> \
      --password <YourStrongPassword!123> \
      --user-attributes Name="email",Value="<test-email@example.com>"
    ```
![User Signup](/images/00/0008.png?featherlight=false&width=90pc)

3.  **Confirm the User as an Admin:**
    * By default, a new user must confirm their email. For testing, you can confirm them immediately as an administrator.
    * Run the following command, using the same email address as before.
    ```bash
    aws cognito-idp admin-confirm-sign-up \
      --user-pool-id $USER_POOL_ID \
      --username <test-email@example.com>
    ```
    Your user is now active and can sign in.

4.  **Confirm the User doesn't need to change passwords:**
    * If in the console, your User has its status set to FORCE_CHANGE_PASSWORD, run the following command
    ```aws cognito-idp admin-set-user-password \
    --user-pool-id <your-user-pool-id> \
    --username <username> \
    --password <password> \
    --permanent
    ```
![Confirm status](/images/00/0009.png?featherlight=false&width=90pc)


### Configuration Complete

Once you have uploaded all your Lambda function code and configured the Cognito user pool,.