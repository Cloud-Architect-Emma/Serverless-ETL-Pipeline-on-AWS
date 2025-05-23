# Serverless ETL Pipeline on AWS — A production-grade, event-driven serverless ETL (Extract, Transform, Load) pipeline built using AWS managed services. It ingests raw customer feedback data from Amazon S3, transforms it via AWS Lambda, stores cleaned records in DynamoDB, and integrates with SNS and CloudWatch for alerting and observability.

## Solution Overview 

This solution demonstrates a decoupled, scalable, and secure real-time pipeline suitable for processing streaming or batch feedback data with minimal operational overhead. It supports fault tolerance, serverless scalability, and real-time alerting.

## Key Components 

• **Amazon S3**: Receives raw `.json/csv` feedback files from external sources; triggers downstream ETL. • **AWS Lambda**: Stateless processor that extracts and transforms raw input into structured output. • **Amazon DynamoDB**: Serverless NoSQL store for processed records with fast query capabilities. • **Amazon SNS**: Delivers ETL failure alerts via email or webhook to notify engineers. • **Amazon CloudWatch**: Captures metrics, logs, and supports alerting dashboards. • **IAM**: Provides fine-grained access control with least privilege principle.

## Project Architecture 

1. User uploads raw feedback data to S3. 2. S3 triggers Lambda on `ObjectCreated` event. 3. Lambda function reads the file, transforms data, and pushes structured results to DynamoDB. 4. If transformation fails or anomalies are detected, Lambda publishes a message to an SNS topic. 5. CloudWatch records logs and custom metrics for monitoring and alerting.

## Architecture Overview

![ETL Architecture](Architecture/ETL%20architecture.jpg)



## AWS Architecture Diagram 

┌────────────┐        ┌────────────┐        ┌────────────┐ │  S3 Bucket │──────▶│   Lambda    │──────▶│  DynamoDB   │ └────────────┘        └────┬───────┘        └────┬───────┘                         │                    │                         ▼                    ▼                ┌────────────┐        ┌──────────────┐                │ CloudWatch │        │     SNS      │                └────────────┘        └──────────────┘

## Project Snapshots

| Component         | Screenshot                                                                 |
|------------------|----------------------------------------------------------------------------|
| SNS Notification | <img src="Project Snapshots/SNS notification.JPG" width="100"/>            |
| DynamoDB Table   | <img src="Project Snapshots/DynamoDB.JPG" width="100"/>                    |
| S3 Bucket        | <img src="Project Snapshots/S3 bucket.JPG" width="100"/>                   |
| Lambda Function  | <img src="Project Snapshots/Lambda function.JPG" width="100"/>             |
        |
             

## IAM Role Permissions 

Lambda assumes the IAM role `etl-raw-data-processor` with the following minimum permissions: ```json { "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": [ "dynamodb:PutItem", "s3:GetObject", "cloudwatch:PutMetricData", "sns:Publish" ], "Resource": "*" } ] } ``` Replace `"*"` with scoped ARNs in production environments to apply least-privilege security practices.

## CloudWatch Metrics 

| Metric Name         | Description                                 | |----------------------|---------------------------------------------| | `ProcessedItemCount` | Number of items successfully transformed    | | `ProcessingErrors`   | Count of transformation or ingestion errors | | `ETLLatency`         | Lambda execution duration (ms)              |

## SNS Alerting Flow 

• Topic Name: `sns-test` • Trigger: Lambda errors or malformed input • Subscription: Email (extendable to Lambda, SMS, HTTP)

## AWS Services Used 

Service         | Purpose                              | |-----------------|--------------------------------------| | Amazon S3       | Raw data ingestion (.csv/json)           | | AWS Lambda      | Data transformation                   | | Amazon DynamoDB | Structured, indexed data storage      | | Amazon CloudWatch | Logging and monitoring               | | Amazon SNS      | Alerting and notification             | | IAM             | Execution role and access control     |

## Recommended Folder Structure 

``` . ├── lambda_function.py ├── README.md ├── screenshots/ │   ├── SNS notification.JPG │   ├── DynamoDB.JPG │   └── S3 bucket.JPG └── logs/metrics/ ```

## License

This project is licensed under the MIT License. See the `LICENSE` file for more information.
