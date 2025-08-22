---
title : "Configuring the Security Automation"
date : 2025-08-10
weight : 3
chapter : true
pre : " <b> 3. </b> "
---

## Preparation Overview

Before we fully deploy the security automation pipeline, we need to configure a few things

### Contents:

- [3.1 Configuring EventBridge](./3.1-configure-event-bridge/)
- [3.2 Preparing the test event](./3.2-prepare-test-event/)
- [3.3 Setting up the security automation](./3.3-setting-up-automation/)
- [3.4 Testing the security automation pipeline](./3.4-testing-security-pipeline/)

---

###  1 Configuring EventBridge

Editing your Amazon EventBridge rule to make it more specific. This procedure will configure it to trigger your security workflow only for the specific types of finding.

---

###  2. Preparing the test event

Generate sample threat detections in AWS GuardDuty, finding a specific event, and customizing it to target a real Lambda function in our account.

---

###  3. Setting up the security automation

Set up an Athena CloudTrail table, SNS subscription, and Timeline Analysis Lambda

---

###  4. Testing the security automation pipeline
Use sample threat event to trigger our security automation pipeline, verifying each components.

---


