# Lambda

## Summary

Can use Lambda in 2 ways:

- event-driven compute service: code runs in response to events.
- run code in response to HTTP requests from API Gateway OR API Calls using the SDKs.

User => API Gateway => 1 Lambda function - scales automatically ; every user gets their own Lambda function.

## Languages

- Node
- Java
- Python
- C#

## Pricing

Number of requests:
- First million requests are free. $0.2 for every request after.

Duration:
- Execution time to the nearest 100ms. $0.00001667 per GB-second.

## Triggers

- API Gateway
- AWS IoT
- CloudWatch Events
- CloudWatch Logs
- CodeCommit
- Cognito Sync Trigger
- DynamoDB
- Kinesis
- S3
- SNS

## Best practices

https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html

- Avoid using recursive code in your Lambda function, wherein the function automatically calls itself until some arbitrary criteria is met.
