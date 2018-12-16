# CLI

Commands:

- aws configure
- aws ec2 describe-instances
- aws ec2 describe-images
- aws ec2 run-instances (not start-instances)
- aws ec2 terminte-instances

# SDK

## Retries

Error Retries and Exponential Backoff in AWS.

Each AWS SDK implements automatic retry logic. The AWS SDK for Java automatically retries requests, and you can configure the retry settings using the ClientConfiguration class.

- simple retry
- exponential backoff. The idea behind exponential backoff is to use progressively longer waits between retries for consecutive error responses.

Jitter adds randomness.
