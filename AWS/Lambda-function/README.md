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

### Common Use Cases

* Backend APIs
* ETL (Extract, Transform, Load) jobs
* Automation tasks
* Scheduled (cron) jobs
* Event-driven applications
* Microservices

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

| **Use Case**                | ** Explanation**                                                          | **Why Useful**                                  |
| --------------------------- | -------------------------------------------------------------------------- | ---------------------------------------------- |
| **File Processing**         | Image upload to S3 → Lambda automatically resizes/compresses it            | Automates content processing for websites/apps |
| **Web Backend / API**       | Frontend calls API → Lambda fetches data from DynamoDB/RDS → responds      | Serverless APIs, less cost and maintenance     |
| **Real-time Notifications** | New order or DB change → Lambda triggers → send email/SMS via SNS          | Instant alerts and monitoring                  |
| **Scheduled Tasks**         | Daily backup, report generation, cleanup → triggered via CloudWatch Events | Automates repetitive tasks without servers     |
| **Data ETL Pipelines**      | S3 → Lambda → transform CSV to JSON → store in Redshift/DynamoDB           | Automates data processing for analytics        |

---

## **Pros (Advantages)**

| **Advantage** | **Explanation** |
|---------------|-----------------|
| Serverless | No server management needed; focus only on code |
| Automatic Scaling | Handles thousands of requests automatically |
| Pay-per-use | Only pay for execution time and resources consumed |
| Event-driven | Reacts instantly to events like uploads, DB changes, or API calls |
| Easy Integration | Works with AWS services like S3, DynamoDB, SNS, API Gateway, CloudWatch |
| Flexible | Supports multiple languages: Python, Node.js, Java, Go, etc. |


## **Cons (Limitations)**

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

