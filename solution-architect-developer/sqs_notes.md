# SQS

SQS was the very first service on the AWS platform.

- messages are 256KB
- queued for 1 min to 14 days (default 4 days)
- guaranteed delivery

## standard queues

- best effort ordering, message delivered at least once
- can be delivered multiple times and in any order.

## FIFO queues

- good for banking transactions. Ordering guaranteed, no duplicates.

## SQS Visibility Timeout

- could result in a message being delivered twice
- default 30 seconds, maximum 12 hours.

The timeout can be too low to process a message. If so use the *ChangeVisibilityTimeout* to specify a new timeout. But quiz said it's *ChangeMessageVisibility*.

## SQL Long Polling

Normal behaviour is short polling. Returns immediately even if the queue is empty.
Long polling waits for the message to arrive or for the long poll timeout to end.

Max long poll timeout = 20 s.

## SQS Fanning Out

Use SNS topics to fan out to multiple SQS queues.

## Message retention

You can use the *MessageRetentionPeriod* attribute to set the message retention period from 60 seconds (1 minute) to 1,209,600 seconds (14 days). Default is 4 days. Use *SetQueueAttributes* action.