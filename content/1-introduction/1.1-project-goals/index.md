---
title : "Project Goals"
date : 2025-08-10
weight : 2
chapter : false
pre : " <b> 1.1 </b> "
---

**Content:**
- [🎯 General Objective](#-general-objective)
- [⏱️ Specific Objectives](#️-specific-objectives)
  - [1. Reduce Mean Time to Respond (MTTR)](#1-reduce-mean-time-to-respond-mttr)
  - [2. Automate Forensic Evidence Collection](#2-automate-forensic-evidence-collection)
  - [3. Ensure Consistent and Repeatable Responses](#3-ensure-consistent-and-repeatable-responses)

---

## 🎯 General Objective

The primary goal of this project is to design, build, and deploy a robust, event-driven platform on AWS that automates the end-to-end security incident response lifecycle. This system will transition our security operations from a slow, manual, and reactive model to a proactive, consistent, and near-instantaneous automated model.

---

## ⏱️ Specific Objectives

To achieve our general objective, we will focus on the following specific and measurable goals.

### 1. Reduce Mean Time to Respond (MTTR)
Decrease the MTTR for critical Lambda-based security threats from the standard baseline of **4-6 hours** to **under 60 seconds**. This will be achieved by creating a fully automated workflow that handles threat detection, containment, and initial investigation without requiring human intervention.

### 2. Automate Forensic Evidence Collection
Implement an automated process to collect a comprehensive set of forensic evidence for every triggered incident. The collected artifacts will be stored securely in an immutable S3 bucket and will include:
* The compromised function's source code and configuration.
* A detailed snapshot of the function's IAM role and all attached policies.
* Recent CloudWatch log entries.
* A chronological timeline analysis of related API calls from AWS CloudTrail.

### 3. Ensure Consistent and Repeatable Responses
Codify our incident response procedures into an AWS Step Functions state machine. This ensures that every response to a specific threat type follows the exact same pre-approved, best-practice runbook, eliminating human error and variability.
