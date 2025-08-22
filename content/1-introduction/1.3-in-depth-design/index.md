---
title : "In Depth Design"
date : 2025-08-10
weight : 4
chapter : false
pre : " <b> 1.3 </b> "
---

This document provides a detailed breakdown of the two main components of this project: the core serverless application and the automated security pipeline that protects it.

**Content:**
- [🏗️ Core Application Architecture](#️-core-application-architecture)
  - [Component Breakdown](#component-breakdown)
  - [Typical Request Flow](#typical-request-flow)
- [🛡️ Automated Security Pipeline](#️-automated-security-pipeline)
  - [The Incident Response Lifecycle](#the-incident-response-lifecycle)

---
## 🏗️ Core Application Architecture

The underlying application is a modern, serverless e-commerce backend built entirely on managed AWS services. This design ensures high availability, automatic scaling, and cost-efficiency by eliminating the need for traditional server management.



### Component Breakdown

* **Amazon API Gateway:** This is the "front door" for all incoming client requests. It manages API endpoints (e.g., `/products`), handles request routing, and is responsible for initial request validation and authorization.
* **AWS Cognito:** A secure, managed user identity service. It integrates directly with API Gateway to handle all user authentication (sign-up and sign-in) and provides authorization by issuing JWT tokens that must be included in every API request.
* **AWS Lambda (Application Layer):** These are the microservices that contain the business logic. Each function is responsible for a specific task:
    * `ProductService`: Handles creating new products.
    * `ScanService`: Handles retrieving a list of all products.
    * `RemoveProduct`: Handles deleting a specific product.
* **Amazon DynamoDB:** A fully managed NoSQL database that serves as the persistent data store for the application. It holds all product information and is accessed directly by the Lambda functions.

### Typical Request Flow (Creating a Product)
1.  A registered user signs in via the client application, receiving a JWT token from **Cognito**.
2.  The client sends a `POST` request to the `/products` endpoint on **API Gateway**, including the JWT token in the `Authorization` header.
3.  **API Gateway** uses its Cognito authorizer to validate the token.
4.  If the token is valid, API Gateway forwards the request to the **`ProductService`** Lambda function.
5.  The `ProductService` Lambda executes its logic, validates the input, and writes the new product data to the **DynamoDB** table.
6.  A success response is returned through the same path to the client.

---
## 🛡️ Automated Security Pipeline

The security pipeline is an event-driven workflow designed to automatically detect, contain, investigate, and report on security threats within the AWS account, requiring zero human intervention for the initial response.

### The Incident Response Lifecycle

The pipeline operates as a five-stage automated runbook, orchestrated by **AWS Step Functions**.

#### **1. Detection**
* **Service:** **AWS GuardDuty**
* **Role:** GuardDuty is the threat detection engine. It continuously monitors AWS log sources (like CloudTrail and VPC Flow Logs) for malicious or unauthorized behavior. When it identifies a potential threat, such as a Lambda function communicating with a malicious IP, it generates a **finding**.

#### **2. Trigger**
* **Service:** **Amazon EventBridge**
* **Role:** EventBridge acts as the central nervous system. It has a rule configured to listen for specific findings from GuardDuty. When a finding matches the rule's pattern, EventBridge immediately triggers the Step Functions state machine to begin the response.

#### **3. Orchestration & Response**
* **Service:** **AWS Step Functions & AWS Lambda**
* **Role:** The Step Functions state machine executes the pre-defined, multi-step incident response plan. Each step is a dedicated Lambda function.
    1.  **Containment (`ContainmentLambda`):** The first action is to immediately stop the potential threat. This function gets the ARN of the compromised Lambda from the GuardDuty finding and attaches a restrictive IAM "deny-all" policy to its role, revoking its permissions to do anything else. **MTTR: < 60 seconds.**
    2.  **Evidence Collection (`EvidenceCollectionLambda`):** This function acts as a digital forensics expert. It collects a snapshot of the compromised function's state, including its source code, configuration, Lambda Layers, and a detailed breakdown of its IAM role and policies. All evidence is saved to a secure S3 bucket.
    3.  **Timeline Analysis (`TimelineAnalysisLambda`):** This function automates the work of an investigator. It uses **Amazon Athena** to automatically query **AWS CloudTrail** logs for all API activity performed by the compromised function's role around the time of the incident, creating a clear timeline of events.
    4.  **Reporting (`GenerateReportLambda`):** This function acts as a security analyst. It reads all the collected evidence (configuration, IAM policies, logs, and timeline) from S3 and compiles a single, human-readable report.
    5.  **Notification (`Amazon SNS`):** The final state of the Step Function takes the formatted report and publishes it to an **Amazon SNS** topic, which then sends it to subscribers (e.g., via email) to alert the human security team of the threat and the automated actions that were taken.

By the time a human analyst sees the notification, the threat has already been contained, and a complete folder of evidence and a summary report are ready for their review.