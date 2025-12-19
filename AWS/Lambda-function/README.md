# AWS Lambda

## What is AWS Lambda?

AWS Lambda is a **serverless compute service** provided by AWS that allows you to run code without provisioning or managing servers.

You simply upload your code, configure a trigger, and AWS Lambda automatically executes the code whenever the event occurs. You are charged only for the execution time.

---

## Why use AWS Lambda?

AWS Lambda is widely used because it:

* Eliminates the need to manage servers
* Automatically scales based on incoming requests
* Reduces cost (pay only for execution time)
* Supports event-driven architecture
* Integrates easily with other AWS services

---

## AWS Lambda Language Support

AWS Lambda supports multiple programming languages:

* Python
* Node.js
* Java
* Go
* .NET (C#)
* Ruby

It also supports **custom runtimes** using Docker container images or Amazon Linux.

---

## Event-Driven Execution

### What is Event-Driven Execution?

Event-driven execution means that code runs **only when an event happens**, not continuously.

An **event** can be:

* File upload (Amazon S3)
* API request
* Message arrival (SQS / SNS)
* Database change
* Scheduled time (cron job)

**No event = no execution**

## Event-Driven Execution in AWS Lambda

* Lambda functions **do not run all the time**
* They run **only when an event triggers them**

### Execution Flow

```
Event happens
   ↓
AWS service triggers Lambda
   ↓
Lambda executes code
   ↓
Execution ends
```

## AWS Lambda Triggers

AWS Lambda functions can be triggered by:

* Amazon S3 (file upload events)
* API Gateway (HTTP/API requests)
* EventBridge / CloudWatch (scheduled jobs)
* Amazon SQS / SNS (message-based triggers)
* DynamoDB Streams (data change events)
* Amazon Cognito (authentication events)
---

## AWS Lambda Limitations


| Limitation             | Description                             |
| ---------------------- | --------------------------------------- |
| Maximum execution time | 15 minutes                              |
| Stateless              | No data is preserved between executions |
| Cold start latency     | Initial invocation may be slow          |
| Memory limit           | 128 MB to 10 GB                         |
| Package size limit     | 50 MB (ZIP), 10 GB (container image)    |
| Not suitable for       | Long-running or heavy workloads         |

---

## Use Cases of Lambda

| Use Case                    | Explanation                                                                | Why Useful                                  |
| --------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------- |
| **File Processing**         | Image upload to S3 → Lambda automatically resizes/compresses it            | Automates content processing for websites/apps |
| **Web Backend / API**       | Frontend calls API → Lambda fetches data from DynamoDB/RDS → responds      | Serverless APIs, less cost and maintenance     |
| **Real-time Notifications** | New order or DB change → Lambda triggers → send email/SMS via SNS          | Instant alerts and monitoring                  |
| **Scheduled Tasks**         | Daily backup, report generation, cleanup → triggered via CloudWatch Events | Automates repetitive tasks without servers     |
| **Data ETL Pipelines**      | S3 → Lambda → transform CSV to JSON → store in Redshift/DynamoDB           | Automates data processing for analytics        |

---

## Pros 

| **Advantage** | **Explanation** |
|---------------|-----------------|
| Serverless | No server management needed; focus only on code |
| Automatic Scaling | Handles thousands of requests automatically |
| Pay-per-use | Only pay for execution time and resources consumed |
| Event-driven | Reacts instantly to events like uploads, DB changes, or API calls |
| Easy Integration | Works with AWS services like S3, DynamoDB, SNS, API Gateway, CloudWatch |
| Flexible | Supports multiple languages: Python, Node.js, Java, Go, etc. |


## Cons 

| **Limitation** | **Explanation** |
|----------------|-----------------|
| Execution Timeout | Max 15 minutes per invocation; not suitable for long-running tasks |
| Cold Start Latency | First invocation may be slower if function hasn't run recently |
| Limited Resources | Max 10 GB memory, limited disk space (/tmp folder) |
| Complex Debugging | Harder to debug compared to traditional servers |
| Vendor Lock-in | Deep AWS integration can make switching to another cloud difficult |
| Stateless Nature | Functions are stateless; need external storage for persistent state |

---
## Best Practices for AWS Lambda

### Code Best Practices

* Keep functions small and single-purpose
* Use environment variables for configuration
* Handle errors and exceptions properly
* Avoid hardcoding secrets

### Performance Best Practices

* Allocate appropriate memory
* Reuse database and SDK connections
* Minimize dependencies
* Optimize cold start times

### Security Best Practices

* Follow IAM least privilege principle
* Store secrets in AWS Secrets Manager or Parameter Store
* Enable logging and monitoring

### Monitoring & Logging

* Use Amazon CloudWatch for logs
* Configure alarms for failures and errors


---
# S3 to S3 File Copy using AWS Lambda (Event-Based)

## Overview

This project demonstrates how to create an **event-driven AWS Lambda function** that automatically copies files from one S3 bucket (source) to another S3 bucket (destination).

Whenever a file is uploaded to the **source S3 bucket**, the Lambda function is triggered automatically. The function reads the file name from the S3 event and uploads the same file to the destination bucket using `boto3`.

---

## Architecture Flow

1. A file is uploaded to the **source S3 bucket**
2. S3 generates an **ObjectCreated (PUT) event**
3. The event triggers the **AWS Lambda function**
4. Lambda reads the file name from the event
5. Lambda copies the file to the **destination S3 bucket**

```
S3 (Source Bucket)
        ↓
   Lambda Function
        ↓
S3 (Destination Bucket)
```

---

## Prerequisites

* AWS Account
* Two S3 buckets created:

  * Source Bucket (example: `bucket-source-2720`)
  * Destination Bucket (example: `bucket-dest-8475`)
* AWS Lambda function with Python runtime
* Basic understanding of AWS S3 and Lambda

---

## Step 1: Create the Lambda Function

1. Go to **AWS Console → Lambda**
2. Click **Create function**
3. Choose **Author from scratch**
4. Runtime: **Python 3.x**
5. Create the function

---

## Step 2: Configure IAM Permissions

Attach the following permissions to the Lambda execution role:

* Read objects from the source bucket
* Write objects to the destination bucket

Example permissions:

* `s3:GetObject` on source bucket
* `s3:PutObject` on destination bucket

---

## Step 3: Add the Lambda Code

Paste the following code into the Lambda function and deploy it:

```python
import boto3
import urllib.parse

s3 = boto3.client('s3')

DEST_BUCKET = "bucket-dest-8475"

def lambda_handler(event, context):

    # Get source bucket name from S3 event
    source_bucket = event['Records'][0]['s3']['bucket']['name']

    # Get file name (object key) from S3 event
    object_key = urllib.parse.unquote_plus(
        event['Records'][0]['s3']['object']['key']
    )

    # Read file from source bucket
    response = s3.get_object(
        Bucket=source_bucket,
        Key=object_key
    )

    file_data = response['Body'].read()
    content_type = response.get('ContentType', 'binary/octet-stream')

    # Upload file to destination bucket
    s3.put_object(
        Bucket=DEST_BUCKET,
        Key=object_key,
        Body=file_data,
        ContentType=content_type
    )

    return {
        "statusCode": 200,
        "message": "File copied successfully"
    }
```

---

## Step 4: Configure S3 Event Trigger

1. Open the **source S3 bucket**
2. Go to **Properties → Event notifications**
3. Create a new event notification
4. Event type: **Object Created (PUT)**
5. Destination: **Lambda function**
6. Select your Lambda function
7. Save the configuration

---

## Step 5: Testing the Setup

1. Upload any file to the source bucket
2. The Lambda function will trigger automatically
3. Check the destination bucket
4. The same file will appear in the destination bucket

---

## Use Case

* Data replication between buckets
* File preprocessing pipelines
* Backup automation
* Event-driven data engineering workflows

---

##  Lambda Resources

➡️ [Download File from Google Drive](https://drive.google.com/file/d/1MhZ6RguGRGzPeki4pWVfhI3SpgmCPGB4/view?usp=drivesdk)

