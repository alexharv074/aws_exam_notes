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

## Default timeout

3 seconds.

## Step functions

AWS Step Functions is a web service that enables you to coordinate the components of distributed applications and microservices using visual workflows. You build applications from individual components that each perform a discrete function, or task, allowing you to scale and change applications quickly. Step Functions provides a reliable way to coordinate components and step through the functions of your application. Step Functions provides a graphical console to visualize the components of your application as a series of steps. It automatically triggers and tracks each step, and retries when there are errors, so your application executes in order and as expected, every time. Step Functions logs the state of each step, so when things do go wrong, you can diagnose and debug problems quickly.

Here are some of the key features of AWS Step Functions:

- Step Functions is based on the concepts of tasks and state machines.
- You define state machines using the JSON-based Amazon States Language.
- The Step Functions console displays a graphical view of your state machine's structure, which provides you with a way to visually check your state machine's logic and monitor executions.

## Debugging

### Logging statements

You can insert logging statements into your code to help you validate that your code is working as expected. Lambda automatically integrates with Amazon CloudWatch Logs and pushes all logs from your code to a CloudWatch Logs group associated with a Lambda function (/aws/lambda/<function name>).

### Dead-letter queues

Any Lambda function invoked asynchronously is retried twice before the event is discarded. If the retries fail and you're unsure why, use Dead Letter Queues (DLQ) to direct unprocessed events to an Amazon SQS queue or an Amazon SNS topic to analyze the failure.

### AWS X-Ray

AWS X-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization.

## Lambda@Edge

Lambda@Edge is an extension of AWS Lambda, a compute service that lets you execute functions that customize the content that CloudFront delivers.

# API Gateway

## Set up API Methods in API Gateway

In API Gateway, an API method embodies a method request and a method response. You set up an API method to define what a client should or must do to submit a request to access the service at the backend and to define the responses that the client receives in return. For input, you can choose method request parameters, or an applicable payload, for the client to provide the required or optional data at run time. For output, you determine the method response status code, headers, and applicable body as targets to map the backend response data into, before they are returned to the client. To help the client developer understand the behaviors and the input and output formats of your API, you can document your API and provide proper error messages for invalid requests.

## Deploying an API

To deploy an API, you create an API deployment and associate it with a stage. Each stage is a snapshot of the API and is made available for the client to call. Every time you update an API, which includes modification of methods, integrations, authorizers, and anything else other than stage settings, you must redeploy the API to an existing stage or to a new stage. As your API evolves, you can continue to deploy it to different stages as different versions of the API. You can also deploy your API updates as a canary release deployment, enabling your API clients to access, on the same stage, the production version through the production release, and the updated version through the canary release.

### Stage variables

Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of an API. They act like environment variables and can be used in your API setup and mapping templates.
