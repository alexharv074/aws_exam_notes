# AWS CodeCommit

## Cross-account access

You can configure access to AWS CodeCommit repositories for IAM users and groups in another AWS account. This is often referred to as cross-account access.

# AWS CodeBuild

## Overview

- AWS CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.
- CodeBuild scales continuously and processes multiple builds concurrently, so your builds are not left waiting in a queue.
- With CodeBuild, you are charged by the minute for the compute resources you use.

## buildspec.yml

Store a build spec file somewhere other than the root of your source directory, such as config/buildspec.yml.

### Commands

- `aws codebuild start-build`: start the build.

### Parameters

- `buildspecOverride`: Optional string. A build spec declaration that overrides for this build the one defined in the build project. If this value is set, it can be either an inline build spec definition or the path to an alternate build spec file relative to the value of the built-in CODEBUILD_SRC_DIR environment variable.

# AWS CodePipeline

## Configuration

### Security

There are two ways to configure server-side encryption for Amazon S3 artifacts:

- AWS CodePipeline creates an Amazon S3 artifact bucket and default AWS-managed SSE-KMS encryption keys when you create a pipeline using the Create Pipeline wizard. The master key is encrypted along with object data and managed by AWS.

- You can create and manage your own customer-managed SSE-KMS keys.

## Cross-account

- Create a Code Pipeline instance in another account
- Add a cross-account role

# AWS CodeDeploy

## Lifecycle event hooks

- ApplicationStop
- DownloadBundle
- BeforeInstall
- Install
- AfterInstall
- ApplicationStart
- ValidateService
- BeforeBlockTraffic
- BlockTraffic
- AfterBlockTraffic
- BeforeAllowTraffic
- AllowTraffic
- AfterAllowTraffic

## AppSpec file

An application specification file (AppSpec file), which is unique to AWS CodeDeploy, is a YAML-formatted or JSON-formatted file. The AppSpec file is used to manage each deployment as a series of lifecycle event hooks, which are defined in the file.

## Deployments

There are three ways traffic can shift during a deployment:

- Canary: Traffic is shifted in two increments. You can choose from predefined canary options that specify the percentage of traffic shifted to your updated Lambda function version in the first increment and the interval, in minutes, before the remaining traffic is shifted in the second increment.
- Linear: Traffic is shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic shifted in each increment and the number of minutes between each increment.
- All-at-once: All traffic is shifted from the original Lambda function to the updated Lambda function version all at once.

# AWS CodeStar

AWS CodeStar is a cloud-based service for creating, managing, and working with software development projects on AWS. You can quickly develop, build, and deploy applications on AWS with an AWS CodeStar project. An AWS CodeStar project creates and integrates AWS services for your project development toolchain.

Seems like it's a kind of wrapper around CodeCommit, CodePipeline etc.
