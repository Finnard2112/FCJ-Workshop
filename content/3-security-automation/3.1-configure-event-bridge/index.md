---
title : "Configuring EventBridge"
date : 2025-08-10
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

This sections walks through the process of editing your Amazon EventBridge rule to make it more specific. By default, the rule might be broad. This procedure will configure it to trigger your security workflow **only** for the specific types of finding.

### Step 1: Navigate to Amazon EventBridge

In the AWS Management Console, type "EventBridge" in the search bar and select it.

### Step 2: Select Your Rule

1.  In the left-hand navigation pane, click on **Rules**.
2.  From the list of rules, select the one you created for this project (e.g., `GuardDuty-Lambda-Threat-Listener`).


![Rule](/images/00/0013.png?featherlight=false&width=90pc)


### Step 3: Edit the Rule

In the top-right corner of the rule's detail page, click the **Edit** button.


![Edit Rule](/images/00/0014.png?featherlight=false&width=90pc)

### Step 4: Edit the Event Pattern

1.  On the "Edit rule" page, scroll down to the **Event pattern** section.
2.  Click the **Edit pattern** button. This will open an editor where you can modify the JSON that defines what the rule listens for.
3.  You will now update the JSON pattern to filter for the specific finding type.

    * **Original Pattern (Broad):**
        ```json
        {
          "source": ["aws.guardduty", "custom.test.guardduty"],
          "detail-type": ["GuardDuty Finding"]
        }
        ```

    * **New Pattern (Specific):**
        Replace the original pattern with the following JSON. This adds a `detail` block to match the specific `type` of the finding. For example, we will use `UnauthorizedAccess:Lambda/TorRelay` for now.
          
        ```json
        {
          "source": ["aws.guardduty", "custom.test.guardduty"],
          "detail-type": ["GuardDuty Finding"],
          "detail": {
            "type": ["UnauthorizedAccess:Lambda/TorRelay"]
          }
        }
        ```

![Edit Rule JSON](/images/00/0015.png?featherlight=false&width=90pc)

### Step 5: Save the Changes

1.  After pasting the new pattern, scroll to the bottom of the section and click **Next**.
2.  You do not need to change the target. Click **Next** again.
3.  On the final review page, click **Update rule**.

---
### Configuration Complete

Your security automation workflow is now configured to only respond to the specific `UnauthorizedAccess:Lambda/TorRelay` threat, making your automation more precise.