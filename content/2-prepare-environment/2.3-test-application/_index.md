---
title : "Testing the Underlying Application"
date : 2025-08-10
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

We will test the underlying application before further implementing security automations to protect it. The API Gateway is authorized by a Cognito User Pool and GuardDuty at this point.

### Prerequisites

Before you begin, ensure you have:
* **Python 3** installed on your machine.
* The **Boto3** and **Requests** Python libraries installed. If not, run:
  ```bash
  pip install boto3 requests
* **Download** the testing file from the following link: https://github.com/Finnard2112/Internship/tree/intern/Nguyen-Ha-Phan/project/Application_Tests.py
* **Information** related to the test user you created

### Step 1: Configure the Test Script
Open the Application_Tests.py script in a code editor and update the constants at the top of the file to match your deployed environment.

* USER_POOL_ID and CLIENT_ID: Replace the placeholder values with the UserPoolId and UserPoolClientId from your CloudFormation stack's Outputs tab or Cognito's console.

* REGION: Ensure this matches the AWS Region where you deployed your stack.

* USERNAME and PASSWORD: Update these with the credentials of the test user you created in Cognito.

* API_URL: Replace the placeholder URL with the ApiUrl from your CloudFormation stack's Outputs tab. Make sure it points to the correct resource (e.g., .../prod/products).

**Example Configuration:**
```
USER_POOL_ID = "us-east-1_xxxxxxxxx"
CLIENT_ID = "xxxxxxxxxxxxxxxxxxxxxxxxxx"
REGION = "us-east-1"
USERNAME = "your-test-user@example.com"
PASSWORD = "YourStrongPassword!123"
API_URL = "[https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/prod/products](https://xxxxxxxxxx.execute-api.us-east-1.amazonaws.com/prod/products)"
```

![Test File](/images/00/0010.png?featherlight=false&width=90pc)

### Step 2: Running the Tests

The Python script is organized into functions that test different parts of your API. The main execution block at the bottom of the script controls which test is run.

#### **A. Test Authentication (`get_cognito_tokens`)**

This function authenticates with Cognito using the provided username and password to retrieve JWT tokens. This is the first step for all other tests.

----
#### **B. Test Fetching Products (`get_products`)**

This is the default test run by the script. It uses the ID token from Cognito to make an authorized GET request to your /products endpoint.

* Make sure that ```api_response = get_products(tokens["IdToken"])``` on line 138
* **To Run:** Execute the script from your terminal without any changes.
  ```bash
  python Application_Tests.py
  ```

**Expected Output**: You will see a "✅ API Call Successful!" message and a JSON response containing a list of products (which will be empty initially).\

![GetProducts](/images/00/0011.png?featherlight=false&width=90pc)

----

#### **C. Test Creating a Product (`create_product`)**

This function sends an authorized POST request with a sample product payload to create a new item in your DynamoDB table.

* Make sure that ```api_response = create_product(tokens["IdToken"])``` on line 138
* **To Run:** Execute the script from your terminal with the following changes.
  ```bash
  python Application_Tests.py
  ```

**Expected Output**: A success message and a JSON response confirming the product was created, which may include the new productId. **Note the productId**


#### **D. Test Deleting a Product (`delete_product`)**

This function sends an authorized DELETE request to remove a product. It requires a valid productId.

* Make sure that ```api_response = create_product(tokens["IdToken"], product_id_to_delete)``` on line 138
* **Replace** product_id_to_delete with the actual product_id you noted
* **To Run:** Execute the script from your terminal with the following changes.
  ```bash
  python Application_Tests.py
  ```
**Expected Output**: A success message and a JSON response confirming the deletion.

By following these steps, you can thoroughly test the authentication and core CRUD (Create, Read, Update, Delete) functionality of the underlying serverless application.

![DeleteProduct](/images/00/0012.png?featherlight=false&width=90pc)