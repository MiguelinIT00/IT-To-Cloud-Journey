# Lambda Basics

## What is AWS Lambda?

AWS Lambda is a serverless compute service that allows you to run code without provisioning or managing servers. It executes code automatically in response to events.

---

## Key Concepts

- **Serverless**

  No need to manage infrastructure or servers

- **Event-Driven**

  Runs code based on triggers (events)

- **Auto Scaling**

  Automatically scales depending on demand

- **Stateless**

  Each execution is independent (no memory of previous runs)

- **Execution Limit**

  Maximum runtime is 15 minutes per invocation

---

## Common Triggers

- S3 → file uploads

- API Gateway → HTTP requests

- DynamoDB → data changes

- CloudWatch → scheduled jobs

---

## How It Works

1. Event occurs (e.g., file uploaded to S3)

2. Lambda function is triggered

3. Code executes

4. Lambda stops after execution completes

---

## Use Cases

- Image processing (resize on upload)

- Backend APIs (via API Gateway)

- Automation scripts

- Data processing pipelines

---

## Pricing

- Pay only when code runs

- Based on:

  - Execution time (milliseconds)

  - Memory usage

No cost when idle

---

## Advantages

- No server management

- Scales automatically

- Cost efficient

- Easy integration with AWS services

---

## Limitations

- 15 minute execution limit

- Cold start delay

- Not ideal for long-running workloads

- Limited environment control

---

## Related Services

- API Gateway → exposes Lambda as an API

- S3 → triggers Lambda events

- DynamoDB → triggers on data updates

- CloudWatch → logging and scheduling
 
