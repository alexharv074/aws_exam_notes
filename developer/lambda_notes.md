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

## Configuration

### Environment variables

Can be used to designate the environment (test, dev etc) for the function. See also https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html

### Encrypted environment variables

When you create or update Lambda functions that use environment variables, AWS Lambda encrypts them using the AWS Key Management Service.

### Versioning

By using versioning, you can manage your in-production function code in AWS Lambda better. When you use versioning in AWS Lambda, you can publish one or more versions of your Lambda function. As a result, you can work with different variations of your Lambda function in your development workflow, such as development, beta, and production.

### Aliases

You can create one or more aliases for your Lambda function. An AWS Lambda alias is like a pointer to a specific Lambda function version.

### Traffic-shifting using aliases

To configure an alias to shift traffic between two function versions based on weights by using the CreateAlias operation, you need to configure the routing-config parameter.
```
aws lambda create-alias --name alias name --function-name function-name function-version 1 \
  --routing-config AdditionalVersionWeights={"2"=0.02}
```

```
aws lambda update-alias --name alias name --function-name function-name \
  --routing-config AdditionalVersionWeights={"2"=0.05}
```

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

The AWS X-Ray daemon is a software application that listens for traffic on UDP port 2000, gathers raw segment data, and relays it to the AWS X-Ray API.

### IAM Role (Execution Role)

If your Lambda function code is executing, but you don't see any log data being generated after several minutes, this could mean your execution role for the Lambda function did not grant permissions to write log data to CloudWatch Logs.

## Lambda@Edge

Lambda@Edge is an extension of AWS Lambda, a compute service that lets you execute functions that customize the content that CloudFront delivers.

## SAM

Steps for Using AWS SAM
The following steps outline how to build a serverless application using AWS SAM:

- Initialize. Download a sample application from template using sam init.

- Test Locally. Test the application locally using sam local invoke and/or sam local start-api. Note that with these commands, even though your Lambda function is invoked locally, it reads from and writes to AWS resources in the AWS Cloud.

- Package. When you're satisfied with your Lambda function, bundle the Lambda function, AWS SAM template, and any dependencies into an AWS CloudFormation deployment package using `sam package`.

- Deploy. Deploy the application to the AWS Cloud using `sam deploy`. At this point, you're able to test your application in the AWS Cloud by invoking it using standard Lambda methods.

# API Gateway

## Set up API Methods in API Gateway

In API Gateway, an API method embodies a method request and a method response. You set up an API method to define what a client should or must do to submit a request to access the service at the backend and to define the responses that the client receives in return. For input, you can choose method request parameters, or an applicable payload, for the client to provide the required or optional data at run time. For output, you determine the method response status code, headers, and applicable body as targets to map the backend response data into, before they are returned to the client. To help the client developer understand the behaviors and the input and output formats of your API, you can document your API and provide proper error messages for invalid requests.

aka "integration request".

## Deploying an API

To deploy an API, you create an API deployment and associate it with a stage. Each stage is a snapshot of the API and is made available for the client to call. Every time you update an API, which includes modification of methods, integrations, authorizers, and anything else other than stage settings, you must redeploy the API to an existing stage or to a new stage. As your API evolves, you can continue to deploy it to different stages as different versions of the API. You can also deploy your API updates as a canary release deployment, enabling your API clients to access, on the same stage, the production version through the production release, and the updated version through the canary release.

### API Stage

A logical reference to a lifecycle state of your API (for example, 'dev', 'prod', 'beta', 'v2'). API stages are identified by API ID and stage name.

### Stage variables

Stage variables are name-value pairs that you can define as configuration attributes associated with a deployment stage of an API. They act like environment variables and can be used in your API setup and mapping templates.

### CORS for API Gateway

When your API's resources receive requests from a domain other than the API's own domain, you must enable cross-origin resource sharing (CORS) for selected methods on the resource. This amounts to having your API respond to the OPTIONS preflight request with at least the following CORS-required response headers:

- Access-Control-Allow-Methods
- Access-Control-Allow-Headers
- Access-Control-Allow-Origin

### Canary release

Canary release is a software development strategy in which a new version of an API (as well as other software) is deployed as a canary release for testing purposes, and the base version remains deployed as a production release for normal operations on the same stage.

## Authorisation

Three methods:

- IAM roles
- Lambda authorizers
- Cognito User Pools

### Lambda authorizer

An API Gateway Lambda authorizer (formerly known as a custom authorizer) is a Lambda function that you provide to control access to your API methods.

### Cognito User Pools

As an alternative to using IAM roles and policies or Lambda authorizers (formerly known as custom authorizers), you can use an Amazon Cognito user pool to control who can access your API in Amazon API Gateway.

To use an Amazon Cognito user pool with your API, you must first create an authorizer of the COGNITO_USER_POOLS type and then configure an API method to use that authorizer.
