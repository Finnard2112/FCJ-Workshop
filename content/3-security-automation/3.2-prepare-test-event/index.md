---
title : "Prepare Test Event"
date : 2025-08-10
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

To accurately test our security automation workflow, we need a realistic event payload. This guide walks through the process of generating sample threat detections in AWS GuardDuty, finding a specific event, and customizing it to target a real Lambda function in our account.

### Step 1: Generate Sample Findings in GuardDuty

First, we will have GuardDuty create a set of safe, mock threat detections.

1.  Navigate to the **AWS GuardDuty** console.
2.  In the left-hand navigation menu, click on **Settings**.
3.  Scroll down to the **Sample findings** section and click the **Generate sample findings** button.

![Generate Samples](/images/00/0016.png?featherlight=false&width=90pc)

This will create a variety of different finding types in your account that you can use for testing.

---
### Step 2: Find and Copy the `TorRelay` Finding JSON

Next, we'll locate the specific finding we want to use as a template and copy its raw JSON data.

1.  In the GuardDuty console, click on **Findings** in the left menu.
2.  In the filter bar at the top, search for the finding type: `UnauthorizedAccess:Lambda/TorRelay`.
3.  Click on the resulting finding to open the details pane on the right.
4.  Click on the Hyperlinked text for `Finding ID`.
5.  A new window will appear with the full JSON of the finding. **Copy the entire content** to your clipboard or a text editor.

![Finding the JSON](/images/00/0017.png?featherlight=false&width=90pc)

---
### Step 3: Modify the JSON to Target Your Lambda

Finally, we will edit the copied JSON to replace the placeholder resource with one of your actual Lambda functions.

1.  Paste the JSON you copied into a text editor.
2.  Locate the `functionArn` key, which is nested inside `detail.resource.lambdaDetails`.
3.  Navigate to the **AWS Lambda** console in a new tab and find one of your application functions (e.g., `YourStackName-ProductService`). Copy its full ARN.
4.  Go back to your text editor and replace the value of `functionArn` with the real ARN you just copied.

**Before (Original JSON from GuardDuty):**
```json
    "lambdaDetails": {
      "functionArn": "arn:aws:lambda:us-east-1:123456789012:function:GeneratedExampleFunction",
      ...
    }
```

**After (Modified):**
```
    "lambdaDetails": {
      "functionArn": "arn:aws:lambda:us-east-1:<YOUR_ACCOUNT_ID>:function:YourStackName-ProductService",
      ...
    }
```

---
### Step 4: Modify the JSON's source
1.  Locate the `source` key.
2.  Edit it to ``` "custom.test.guardduty" ```


![Sample JSON](/images/00/0018.png?featherlight=false&width=90pc)
---

### Ready for Testing
You now have a customized, realistic test event payload. You will use this modified JSON in the next step to manually trigger your EventBridge rule and test the entire security automation workflow.