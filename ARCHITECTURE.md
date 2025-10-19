# System Architecture

## Overview
The Cloud Cost & Security Dashboard follows a serverless architecture pattern using AWS services managed through Terraform.

## Architecture Diagram
![Architecture Diagram](./diagrams/architecture-diagram.txt)

## Data Flow
1. Data Collection: AWS EventBridge triggers daily Lambda function
2. Cost Data: Lambda calls AWS Cost Explorer API for monthly costs
3. Security Data: Lambda calls AWS GuardDuty API for security findings
4. Data Storage: Processed data stored in DynamoDB with TTL
5. Frontend: React dashboard fetches and visualizes data
6. Deployment: Static site hosted on Netlify

## AWS Services Used
- AWS Lambda: Serverless compute for data collection
- DynamoDB: NoSQL database for metrics storage
- S3: Backup and static asset storage
- CloudWatch: Logging and monitoring
- EventBridge: Scheduled task execution
- Cost Explorer: AWS cost data API
- GuardDuty: Security findings API
- IAM: Security and access management

## Infrastructure as Code
All AWS resources are defined and managed using Terraform, including:
- IAM roles and policies with least privilege
- DynamoDB table with on-demand capacity
- Lambda functions with environment variables
- EventBridge rules for scheduling
- S3 buckets with proper access controls

## Security Considerations
- IAM roles follow principle of least privilege
- Data encrypted at rest and in transit
- Public dashboard uses mock data for security
- All resources tagged for cost tracking