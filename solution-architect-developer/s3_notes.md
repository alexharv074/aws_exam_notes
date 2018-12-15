# S3

Object-based storage i.e. stores files so you can't run a database there.

Files can be 0 - 5TB in size. And the storage is unlimted.
What is the largest size file you can transfer to S3 using a PUT operation? 5GB.

Files are stored in "buckets" which are really just folders.

Bucket name is globally unique:

- s3://<region>.amazonaws.com/<bucketname>
- http://<bucketname>.s3-website-<region>.amazonaws.com/ ??

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

# pre-signed URLs

If you want to enable a user to download your private data directly from S3, you can insert a pre-signed URL into a web page before giving it to your user.

# Cross origin resource sharing (CORS)

The following are example scenarios for using CORS:

- Scenario 1: Suppose that you are hosting a website in an S3 bucket. Your users load the static content in S3 bucket A. Now you want to use JavaScript on the webpages that are stored in this bucket to be able to make authenticated GET and PUT requests against the same bucket by using the S3 API endpoint for the bucket, S3 bucket B. A browser would normally block JavaScript from allowing those requests, but with CORS you can configure your bucket to explicitly enable cross-origin requests from website.s3-website-us-east-1.amazonaws.com.

- Scenario 2: Suppose that you want to host a web font from your S3 bucket. Again, browsers require a CORS check (also called a preflight check) for loading web fonts. You would configure the bucket that is hosting the web font to allow any origin to make these requests.

> You are hosting a static website in an S3 bucket that uses Java script to reference assets in another S3 bucket. For some reason, these assets are not displaying when users browse to the site. What could be the problem? A: CORS not enabled.

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

# Security & Encryption

By default buckets are PRIVATE.

You can use:

- Bucket policies
- ACLs

You can configure ACCESS LOGS.

## Encryption

In transit:

- SSL/TLS

At rest:

Server-side:

- SSE-S3 requires that Amazon S3 manage the data and master encryption keys.
- SSE-C requires that you manage the encryption key.
- SSE-KMS requires that AWS manage the data key but you manage the master key in AWS KMS. Also gives you an audit trail.

Encryption is AES-256.

Client-side:

You encrypt the data on your side then upload it.

You can specify a condition in the bucket policy to enforce encryption:

```json
"Condition": {
  "StringNotEquals": {
    "s3:x-amz-server-side-encryption": "aws:kms"
  }
}
```

# Storage Gateway

How to backup data in enterprise?

- Write backup data to s3 directly.
- Write backup data to STORAGE GATEWAY, which securely replicates to S3.

Storage Gateway 2017 Old names:

- File Interface
- Volume Interface
    * Gateway-cached volumes
    * Gateway-stored volumes
- Tape Interface
    * Gateway-Virtual Tape Library

File volumes or "File Gateway": NFS - flat files stored in S3. Max file size 5TB.

Volume Gateway (iSCSI):

- Cached (Gateway-cached volumes or "Cached Volumes"): iSCSI - cache frequently used data on-prem and async backup the rest to S3. 1 Vol can store 32TB. 32 Volumes allowed. Max 1PB.
- Stored (Gateway-stored volumes or "Stored Volumes"): iSCSI - store all data on-prem and snapshot to S3. 1 Vol can store 16TB, 32 Vols supported, Max 512TB.

Tape Gateway (Gateway-Virtual Tape Library): iSCSI based virtual tape.
- Backed by S3  - 1500 Virtual Tapes (1PB). Instant retrieval.
- Virtual Tape Shelf backed by Glacier. (unlimated). 24 hour retrieval time.

ports:
- 443
- 80 (activation only)
- 3260 for iSCSI
- udp 53

# Multipart Upload

https://docs.aws.amazon.com/AmazonS3/latest/dev/uploadobjusingmpu.html

Multipart upload allows you to upload a single object as a set of parts. Each part is a contiguous portion of the object's data. You can upload these object parts independently and in any order. If transmission of any part fails, you can retransmit that part without affecting other parts. After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object. In general, when your object size reaches 100 MB, you should consider using multipart uploads instead of uploading the object in a single operation.
