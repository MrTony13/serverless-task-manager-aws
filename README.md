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

serverless-task-manager/
│
├── README.md
│
├── architecture/
│   └── architecture-overview.md
│
├── frontend/
│   └── App.js
│
├── lambda/
│   ├── createTask.js
│   ├── getTasks.js
│   ├── updateTask.js
│   └── deleteTask.js
│
├── dynamodb/
│   └── table-design.md
│
├── cognito/
│   └── cognito-setup.md
│
├── amplify/
│   └── amplify-setup.md
│
└── challenges-and-solutions.md

Anthony Atuegwu

