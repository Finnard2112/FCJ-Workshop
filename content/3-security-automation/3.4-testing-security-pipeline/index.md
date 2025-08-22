---
title : "Testing the security pipeline"
date : 2025-08-10
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

This guide provides the final end-to-end test for your security automation pipeline. We will manually send a crafted test event to the Amazon EventBridge event bus to simulate a threat detection, which will trigger your entire Step Functions workflow.

### Step 1: Prepare the Test Event Payload

**Copy the JSON template from 3.2** From following section 3.2, you should have a sample GuardDuty `UnauthorizedAccess:Lambda/TorRelay` finding.

---
### Step 2: Send the event through EventBridge

1.  Navigate to the **Amazon EventBridge** console.
2. In the left menu, go to **Event** buses.
3. Select the `default` event bus.
4. In the top right, click the **Send** events button.
5. Fill out the form with the following details:
    * Event source: custom.test.guardduty
    * Detail type: GuardDuty Finding
    * Event detail: Paste the detail field's value from the payload's JSON here
6. Click Send.

![Send Event](/images/00/0022.png?featherlight=false&width=90pc)

---
### Step 3: Verify the success of the pipeline
After sending the event, the entire workflow should execute within about a minute. To confirm success, check the following:
1. ✅ Step Function Execution: Navigate to the Step Functions console. Look for a new execution of your state machine with a "Succeeded" status. You can click on it to see the visual graph of the successful workflow.
2. ✅ SNS Notification: Check your email for the formatted incident report sent from your SNS topic.
3. ✅ IAM Containment: Go to the IAM console and inspect the role of the Lambda you targeted (e.g., ProductService). The IncidentResponseDenyAllPolicy should now be attached.
4. ✅ S3 Evidence: Navigate to your secure S3 evidence bucket. A new folder should exist, named after the finding ID, containing all the collected forensic artifacts (configuration.json, iam_permissions.json, timeline_analysis.json, etc.).

![Step Functions](/images/00/0023.png?featherlight=false&width=90pc)

![Report](/images/00/0024.png?featherlight=false&width=90pc)

If you can verify all these results, your security automation pipeline is fully configured and operational.