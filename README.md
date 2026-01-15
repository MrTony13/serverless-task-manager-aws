# serverless-task-manager-aws
Enterprise-grade serverless task manager built with AWS (S3, Amplify, Cognito, API Gateway, Lambda, DynamoDB)
# Serverless AWS Authentication & Data Processing Project
The goal of this project is to show how modern cloud-native applications handle authentication, authorization, and data operations without managing servers.
## Architecture Summary

* **Amazon Cognito** – User authentication and authorization
* **AWS Lambda** – Backend logic and request processing
* **Amazon DynamoDB** – NoSQL database for storing application data
* **Amazon S3** – Optional storage for processed outputs or logs
* **Amazon API Gateway** – Exposes Lambda as a REST API
## Amazon Cognito – Setup Procedure

### Steps Used

1. Created a **User Pool** in Amazon Cognito
2. Configured **sign-in options** using email and password
3. Enabled **self sign-up** for users
4. Set password policy (minimum length, complexity rules)
5. Created an **App Client** (without client secret for Lambda usage)
6. Configured **callback and sign-out URLs**
7. Enabled **Hosted UI** for authentication testing
8. Copied the **User Pool ID** and **App Client ID** for Lambda integration

### Purpose

* Secure authentication
* Token-based access (ID token, Access token)
* Centralized user management
## AWS Lambda – Setup Procedure

### Steps Used

1. Created a Lambda function using **Python 3.x**
2. Assigned an **IAM Role** with permissions for:

   * DynamoDB access
   * CloudWatch Logs
3. Added environment variables:

   * `TABLE_NAME`
   * `AWS_REGION`
4. Integrated Lambda with **API Gateway**
5. Enabled **Cognito Authorizer** in API Gateway
6. Tested the function using test events and API calls
## AWS Lambda – Sample Code

```python
import json
import boto3
import os

# Initialize DynamoDB
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['TABLE_NAME'])

def lambda_handler(event, context):
    try:
        body = json.loads(event['body'])
        user_id = body['user_id']
        data = body['data']

        table.put_item(
            Item={
                'user_id': user_id,
                'data': data
            }
        )

        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Data saved successfully'})
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```


## Amazon DynamoDB – Setup Procedure

### Steps Used

1. Created a DynamoDB table
2. Set **Partition Key** as `user_id` (String)
3. Used **On-Demand Capacity Mode**
4. Disabled auto-scaling (not needed for small project)
5. Verified access permissions from Lambda IAM Role
6. Tested data insertion from Lambda

### Purpose

* Fast, scalable NoSQL storage
* Serverless and fully managed

## Challenges Faced and Solutions

### 1. Lambda Permission Errors

**Problem:** Lambda could not write to DynamoDB

**Solution:**

* Updated IAM Role to include `dynamodb:PutItem`

### 2. Cognito Authorization Failing

**Problem:** API Gateway returned `401 Unauthorized`

**Solution:**

* Correctly attached **Cognito User Pool Authorizer**
* Ensured the Access Token was passed in the `Authorization` header

### 3. JSON Parsing Errors

**Problem:** Lambda failed due to malformed request body

**Solution:**

* Ensured requests were sent as valid JSON
* Added error handling in Lambda code

---

## How to Test the Project

1. Sign up a user using Cognito Hosted UI
2. Log in and copy the **Access Token**
3. Call API Gateway endpoint using Postman or curl
4. Pass the token in the `Authorization` header
5. Verify data appears in DynamoDB

-Key Takeaways

* Fully serverless architecture
* Secure authentication using Cognito
* Scalable backend using Lambda and DynamoDB
* No infrastructure management required

 Author
Anthony Atuegwu

