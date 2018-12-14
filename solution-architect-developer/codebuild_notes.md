# AWS Codebuild

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
