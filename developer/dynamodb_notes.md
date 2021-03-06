# DynamoDB

## Associate notes

Fast - single-digit millisecond latency.
Supports document and key-value.

Stored on SSD storage. Spread across 3 "geographically distinct data centres" - for some reasons, they won't say across 3 AZs.

Consistency models:
- eventual consistent reads. Usually within 1 second.
- strongly consistent reads.

Pricing model is complicated

DynamoDB Streams:
"DynamoDB Streams captures a time-ordered sequence of item-level modifications in any DynamoDB table, and stores this information in a log for up to 24 hours."

## Quick facts

- SSD
- 3 "geographically distinct regions"

Eventual consistency reads - consistency across all copies reached in &lt; 1 second.
Strongly consistent reads - always get the right answer at the expense of performance.

Consists of:

- Tables
- Items (like a row)
- Attributes (like a column)

Pricing:

- write throughput $0.0065 / hour for every 10 units
- read throughput $0.0065 / hour for every 50 units

Schemaless:

Key difference between NoSQL and SQL
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/SQLtoNoSQL.html

First 25GB per month is free.
$0.25GB per month thereafter.

Write Capacity Unit
- 1 WCU = 1 write / second 

Free tier:
- 25 Read Capacity Units
- 25 Write Capacity Units

Along with ElastiCache, a good choice for storing session data (DynamoDB Session Handler).

## Items

### TTL

Time To Live (TTL) for DynamoDB allows you to define when items in a table expire so that they can be automatically deleted from the database.

## Indexes and Streams

Primary keys

- Single Attribute: Parition Key (aka Hash Key) composed of one attribute.
- Composite Attribute: (e.g. unique ID + date) composed of two attributes.

?? Skipped a lot here.

~~~
{
    TableName : "Music",
    KeySchema: [
        {
            AttributeName: "Artist",
            KeyType: "HASH", //Partition key
        },
        {
            AttributeName: "SongTitle",
            KeyType: "RANGE" //Sort key
        }
    ],
    AttributeDefinitions: [
        {
            AttributeName: "Artist",
            AttributeType: "S"
        },
        {
            AttributeName: "SongTitle",
            AttributeType: "S"
        }
    ],
    ProvisionedThroughput: {       // Only specified if using provisioned mode
        ReadCapacityUnits: 1,
        WriteCapacityUnits: 1
    }
}
~~~

The primary key for this table consists of Artist (partition key) and SongTitle (sort key).

Partition:

> DynamoDB stores data in partitions. A partition is an allocation of storage for a table, backed by solid-state drives (SSDs) and automatically replicated across multiple Availability Zones within an AWS Region. Partition management is handled entirely by DynamoDB—you never have to manage partitions yourself.

([ref](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Partitions.html)).

Recommendation for partition key:

> Use high-cardinality attributes. These are attributes that have distinct values for each item, like e-mailid, employee_no, customerid, sessionid, orderid, and so on.

Use case for streams:

- How do you set up a relationship across multiple tables in which, based on the value of an item from one table, you update the item in a second table?

## Global tables

DynamoDB global tables provide a fully managed solution for deploying a multi-region, multi-master database, without having to build and maintain your own replication solution.

## DAX DynamoDB Accelerator

In-Memory Acceleration with DAX. Amazon DynamoDB is designed for scale and performance. In most cases, the DynamoDB response times can be measured in single-digit milliseconds. However, there are certain use cases that require response times in microseconds. For these use cases, DynamoDB Accelerator (DAX) delivers fast response times for accessing eventually consistent data.

Millions of requests per second: DAX provides access to eventually consistent data from DynamoDB tables, with microsecond latency. A multi-AZ DAX cluster can serve millions of requests per second.

## Scan v Query API call

### Query

A Query operation finds items in a table using only the primary key. You must provide the partition name.
Optionally provide a sort key.

To reverse the order, set the *ScanIndexForward* parameter to false.

*ConsistentRead*
Determines the read consistency model: If set to true, then the operation uses strongly consistent reads; otherwise, the operation uses eventually consistent reads.

*ReturnConsumedCapacity*
Set to TOTAL or INDEX to find out about consumed capacity.

### Scan

Scan examines every item in the table. But can use *ProjectionExpression* to limit it.
Less efficient than a Query.

### Parallel scan

By default, the Scan operation processes data sequentially. DynamoDB returns data to the application in 1 MB increments, and an application performs additional Scan operations to retrieve the next 1 MB of data.

### Reduced page size

Because a Scan operation reads an entire page (by default, 1 MB), you can reduce the impact of the scan operation by setting a smaller page size.

## Provisioned throughput

Using the AWS portal, you are trying to Scale DynamoDB past its preconfigured maximums. You can increase provisioned throughput limits by raising a ticket to AWS support.

### Read throughput

Magic formula:

- (Size of read to nearest 4KB / 4) * no of items = read throughput
- divide by 2 if eventually consistent.

Example question:

> You have an application that requires:
>   read 10 items
>   of 1KB / second
> using
>   eventual consistency.
> What do you set the read throughput to?

- Size of read to nearest 4KB is 4 / 4 = *1 read unit per item*
- 10 items * 1 read unit per item = 10
- For eventual consistency 10 / 2 = *4 units of read throughput*

### Write throughput

Writes always 1KB in size and 1 write per second.

> You have an application that requires:
>   write 12 items
>   100KB / second per item.
> What do you set the write throughput to?

12 items * 100KB = 1200 *write units*

### exceed provisioned throughput

HTTP 400 and *ProvsionedThroughputExceededException*.

## Conditional write

To prevent a race condition where two users write to the same item with different values at the same time. Conditional writes are *idempotent*.

## Atomic counters

Increment a counter no matter what using the *UpdateItem* operation. These are not idempotent.

## Batch operations

Use *BatchGetItem*, can return up to 16MB of data / 100 items. Note no s.

## PutItem

Creates a new item, or replaces an old item with a new item.

## Other stuff

- Maximum size of a shard
- Maximum amount of data per primary key

## Encryption of tables

Amazon DynamoDB offers fully managed encryption at rest. DynamoDB encryption at rest provides enhanced security by encrypting all your data at rest using encryption keys stored in AWS Key Management Service (AWS KMS). This functionality helps reduce the operational burden and complexity involved in protecting sensitive data. With encryption at rest, you can build security-sensitive applications that meet strict encryption compliance and regulatory requirements.

Encrypt at time of table creation.

## Fine-grained access control

In DynamoDB, you have the option to specify conditions when granting permissions using an IAM policy (see Access Control). For example, you can:

- Grant permissions to allow users read-only access to certain items and attributes in a table or a secondary index.
- Grant permissions to allow users write-only access to certain attributes in a table, based upon the identity of that user.

## Common errors

This section lists the errors common to the API actions of all AWS services. For errors specific to an API action for this service, see the topic for that API action.

AccessDeniedException
You do not have sufficient access to perform this action.

HTTP Status Code: 400

IncompleteSignature
The request signature does not conform to AWS standards.

HTTP Status Code: 400
