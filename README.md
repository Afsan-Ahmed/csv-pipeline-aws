# CSV Data Pipeline on AWS

## Overview
A serverless AWS pipeline that automatically processes any CSV file uploaded to S3,
stores the data in DynamoDB, tracks AWS costs, and sends a detailed email report
via SNS — all triggered automatically without any manual intervention.

## Architecture
S3 (CSV Upload) → Lambda → DynamoDB → SNS → Email Report

## What it does
- Detects when any CSV file is uploaded to S3
- Automatically downloads and parses the CSV
- Accepts any CSV structure — no hardcoded column names
- Auto-converts numeric values to Decimal for DynamoDB compatibility
- Stores each row into DynamoDB with a unique row_id
- Detects and skips duplicate file uploads
- Calculates estimated AWS cost (Lambda + DynamoDB + S3)
- Sends a detailed email report with cost breakdown and processing stats
- Sends failure alert email if processing fails

## Services Used
- Amazon S3 — CSV file storage and event trigger
- AWS Lambda (Python 3.12) — core processing logic
- Amazon DynamoDB — data storage and cost tracking
- Amazon SNS — email notifications
- Amazon CloudWatch — logging and monitoring

## DynamoDB Tables
| Table | Partition Key | Purpose |
|---|---|---|
| Project-CSV-DYNAMO | row_id (String) | Stores CSV row data |
| COST-Pipeline | file_id (String) | Tracks processing costs |

## Email Report Includes
- File name and bucket
- File size
- Column names detected
- Number of rows processed
- Processing time
- Cost breakdown (Lambda, DynamoDB, S3)
- Total estimated cost

## Cost Tracking
Each file processed is tracked in `COST-Pipeline` table with:
- File ID
- Timestamp
- Rows processed
- Column names
- File size
- Lambda cost
- DynamoDB write cost
- S3 storage cost
- Total estimated cost

## Setup
1. Create two DynamoDB tables:
   - `Project-CSV-DYNAMO` — partition key: `row_id` (String)
   - `COST-Pipeline` — partition key: `file_id` (String)
2. Create an SNS topic and subscribe your email
3. Create a Lambda function with Python 3.12 and paste the code
4. Add S3 trigger to Lambda
5. Grant Lambda the required IAM permissions
6. Upload any CSV to S3 and check your email!

## IAM Permissions Required
- `AmazonS3ReadOnlyAccess`
- `AmazonDynamoDBFullAccess`
- `AmazonSNSFullAccess`
- `CloudWatchLogsFullAccess`

## Improvements Over V1
- Accepts any CSV structure — no hardcoded columns
- Auto-converts numeric values to correct DynamoDB types
- Added S3 storage cost to cost calculation
- Added duplicate file detection
- Enhanced email with full cost breakdown and column info
- Added failure email alert

## Demo
CloudWatch:
<img width="1141" height="492" alt="succ1" src="https://github.com/user-attachments/assets/13e052d8-b892-4112-9bb5-d0e6aba5cd6a" />
<img width="1137" height="483" alt="succ2" src="https://github.com/user-attachments/assets/4fa80cc0-53ce-4331-bde3-381ce2e03706" />

Dynamo:
<img width="1346" height="655" alt="dynamo" src="https://github.com/user-attachments/assets/8cd82295-f70a-4f2c-a8f5-7ea000d4bb96" />



