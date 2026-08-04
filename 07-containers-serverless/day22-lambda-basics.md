# Day 22 — What is Lambda? Serverless Basics

## What it is
**AWS Lambda** lets you run code without provisioning or managing servers — you upload a function, AWS runs it in response to triggers (events), and you're billed only for the compute time actually used.

## Why it matters
Lambda is the cornerstone of "serverless" architecture — no idle server costs, automatic scaling, minimal operational overhead. A key building block for modern event-driven systems.

## Key concepts

### How it works
1. Write a function (Python, Node.js, Java, etc.)
2. Package it and upload to Lambda (or write inline for small functions)
3. Attach a **trigger** — an event source that invokes the function (S3 upload, API Gateway request, DynamoDB stream, scheduled CloudWatch Event, etc.)
4. Lambda runs your code, scales automatically based on incoming events, then shuts down

### Key properties
- **Max execution time**: 15 minutes per invocation
- **Billing**: per millisecond of execution + memory allocated — pay nothing when not running
- **Cold starts**: first invocation after idle time has extra latency while AWS provisions the execution environment
- **Concurrency**: Lambda scales out automatically, running many invocations in parallel

## Hands-on / commands

```bash
# Create a Lambda function from a zipped code package
aws lambda create-function \
  --function-name my-function \
  --runtime python3.12 \
  --role arn:aws:iam::123456789012:role/lambda-execution-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

# Invoke it manually
aws lambda invoke --function-name my-function output.json

# Add an S3 trigger (function runs whenever a file is uploaded)
aws lambda add-permission \
  --function-name my-function \
  --statement-id s3invoke \
  --action lambda:InvokeFunction \
  --principal s3.amazonaws.com \
  --source-arn arn:aws:s3:::my-bucket
```

## Common exam gotchas
- Lambda always needs an **execution role** (IAM role) — even if the function doesn't touch other AWS services, it still needs at least CloudWatch Logs permissions
- 15-minute max duration — long-running processes need a different service (ECS, Step Functions, EC2)
- "Event-driven, no servers, pay-per-execution" in a question almost always points to Lambda

## My notes / things that confused me
a common gotcha worth knowing: when working with KMS-encrypted Lambda payloads, remember to decode with raw binary (base64 -d) rather than text mode, or you'll corrupt the encrypted data
