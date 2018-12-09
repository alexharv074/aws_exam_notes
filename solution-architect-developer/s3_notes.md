# S3

Object-based storage i.e. stores files so you can't run a database there.

Files can be 0 - 5TB in size. And the storage is unlimted.

Files are stored in "buckets" which are really just folders.

Bucket name is globally unique:

- s3://<region>.amazonaws.com/<bucketname>

Successful upload returns HTTP 200.

Data consistency model:

- Read after write for PUTS of new objects
- Eventual consistency for overwrite PUTS and DELETES

Object storage. An object consists of:

- Key (like filename)
- Value (a series of bytes being the data)
- Version ID
- Metadata
- Subresources:
    * ACLs
    * Torrent (for Bit Torrent protocol).

Storage classes:

- S3 - availablility is 99.99%, SLA 99.9% availability, durability is 99.999999999% (11 9s guarantee).
- S3 IA (infrequently accessed) - lower fee, retrieval fee.
- RRS (reduced redundancy storage) - 99.99% availability and durability per year.
- Glacier - Very cheap and 3-5 hours to restore. 1 cent per GB per month.

|    |Standard|IA|RRS|
|----|--------|--|---|
|Durability|99.999999999%|99.999999999%|99.99%|
|Availability|99.99%|99.9%|99.99%|
|Concurrent facility fault tolerance|2|2|1|
|SSL Support|yes|yes|yes|
|First byte latency|ms|ms|ms|
|Lifecycle Management Policies|yes|yes|yes|

S3 v Glacier:

|    |Standard|IA|Glacier|
|----|--------|--|---|
|Durability|99.999999999%|99.999999999%|99.999999999%|
|Availability|99.99%|99.9%|N/A|
|Availability SLA|99.9%|99%|N/A|
|Minimum object size|128KB|N/A|
|Retrieval fee|N/A|per GB|per GB|
|First byte latency|ms|ms|minutes or hours|
|Storage class|object|object|object|
|Lifecycle transitions|yes|yes|yes|

S3 charges:

- storage
- requests
- storage management pricing
- data transfer pricing
- transfer acceleration

Transfer acceleration:

Optimised routing via CLoudFront edge locations to your S3 region.

Read the S3 FAQ.

https://aws.amazon.com/s3/faqs/

# Cross origin resource sharing (CORS)

Not sure what I need to know yet.

# S3 Versioning

- Stores all versions of an object (including all writes and even if you delete an object)
- Once enabled, cannot be disabled - only suspended.
- Integrates with Lifecycle Rules.
- MFA Delete - additional layer of security.

# Cross-region replication

- Versioning must be enabled on source & dest
- Regions must be unique (huh?)
- Files are not automatically replicated in an existing bucket. Subsequented updated files will be replicated automatically. So, best to turn on Cross-region replication when the buckets are empty.
- Cannot replicate to multiple buckets using daisy-chaining.
- Delete markers are replicated.
- But, deleting individual versions or delete markers will not be replicated.

# Lifecycle management

- Can be used in conjunction with versioning.
- Can be applied to current and previous versions.
- Possible to:
    * Transition to IA (128KB files after 30 days).
    * Archive to Glacier (30 days after IA).
    * Permanently delete.
