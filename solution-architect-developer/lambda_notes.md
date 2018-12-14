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

## Environment variables

Can be used to designate the environment (test, dev etc) for the function. See also https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html

# API Gateway

## Deploying an API

To deploy an API, you create an API deployment and associate it with a stage. Each stage is a snapshot of the API and is made available for the client to call. Every time you update an API, which includes modification of methods, integrations, authorizers, and anything else other than stage settings, you must redeploy the API to an existing stage or to a new stage. As your API evolves, you can continue to deploy it to different stages as different versions of the API. You can also deploy your API updates as a canary release deployment, enabling your API clients to access, on the same stage, the production version through the production release, and the updated version through the canary release.

### Stage variables

Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of an API. They act like environment variables and can be used in your API setup and mapping templates.
