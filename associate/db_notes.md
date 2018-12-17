# Databases

## RDS

OLTP

### Database types

- SQL
- MySQL
- PostgreSQL
- Oracle
- Aurora
- MariaDB

DynamoDB - NoSQL
RedShift - OLAP

### Backups

- Automated
- Database Snapshots

### Automated

- return database to a point-in-time within a "retention period" (1-35 days)
- daily snapshots + transaction logs throughout the day to within 1 second.
- stored in S3 with free storage equal to the size of your DB.
- enabled by default
- backups happen within a defined window. expect increased latency during the backup window.

### Snapshots

- always user-initiated
- persist even after the RDS instance is deleted, unlike automated backups.

### Restoring

- a completely new RDS instance with a new DNS endpoint.

### Encryption

- at rest supported by: MySQL, Oracle, SQL Server, PostgreSQL, MariaDB & Aurora.
- uses KMS.
- data & backups are then encrypted at rest.

To encrypt an existing RDS instance:

- create snapshot, copy it, encrypt the copy.

### Multi-AZ

- exact copy of DB in another AZ
- AWS handles replication
- automatic failover
- for DR only, not performance improvement.
- available for: SQL Server, Oracle, MySQL Server, PostgreSQL, MariaDB, (also Aurora).

### Read replicas

- read replicas up to 5 by default
- can also have read replicas of replicas in other AZs or even other regions.
- available for: MySQL Server, PostgreSQL, MariaDB, (Aurora).
- performance improvement, not for DR.
- must have automatic backups on.
- each replica has its own DNS endpoint.
- read replicas CAN have Multi-AZ enabled (new feature).
- can create read replicas of Multi-AZ source databases.
- read replica can be promoted but this breaks replication.

### TDE

Amazon RDS supports using Transparent Data Encryption (TDE) to encrypt stored data on your DB instances running Microsoft SQL Server. TDE automatically encrypts data before it is written to storage, and automatically decrypts data when the data is read from storage.

# DynamoDB

See [here](./solution-architect-developer/dynamodb_notes.md).

# Redshift

Columnar storage also == NoSQL

"Data warehousing is really about adding up columns" "columnar data storage"

Single node - 160GB
Multi-node
- leader node
- up to 128 compute nodes

Columnar storage: columns stored sequentially improving read time.
Columnar compression - 10 times faster: compression also works better if you assume data is sequential.
MPP - massive parallel processing. data auto-distributed.

Pricing complicated.

Security:
- encrypted in transit using SSL
- at rest using AES-256
- takes care of key management (default) OR bring your own keys OR KMS

NOT Multi-AZ.

Enhanced VPC Routing.
https://docs.aws.amazon.com/redshift/latest/mgmt/enhanced-vpc-routing.html

When you provision an Amazon Redshift cluster, it is locked down by default so nobody has access to it. To grant other users inbound access to an Amazon Redshift cluster, you associate the cluster with a security group.

# ElastiCache

In-memory cache

Types:

Memcached
- protocol-compliant with memcached

Redis

Exam tips:
- ElastiCache is a good choice in read-heavy infrequently changing workloads.
- Redshift better if stress is caused by lots of OLAP transactions.
- Choose Redis if you need HA.

Good choice for storing session data.

Caching strategies:
https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html

- Lazy loading: As the name implies, lazy loading is a caching strategy that loads data into the cache only when necessary.
- Write through: The write through strategy adds data or updates data in the cache whenever data is written to the database.
- Adding TTL: Lazy loading allows for stale data, but won't fail with empty nodes. Write through ensures that data is always fresh, but may fail with empty nodes and may populate the cache with superfluous data. By adding a time to live (TTL) value to each write, we are able to enjoy the advantages of each strategy and largely avoid cluttering up the cache with superfluous data.

## Redis sorted sets

Leaderboards, such as the Top 10 scores for a game, are computationally complex, especially with a large number of concurrent players and continually changing scores. Redis sorted sets guarantee both uniqueness and element ordering.

# Aurora

"Combines speed and availability of high-end commercial DBs with simplicity and cost of open source. 5 times faster than MySQL / 1/10th as expensive as Oracle"

Features
- starts at 10GB, auto-provisions, scales in 10GB increments up to 64TB.
- scales up to 32 vCPUs and 244GB mem
- 2 copies of the data in each AZ minimum 3 AZ ie 6 copies.
- handles loss of 2 copies without affecting read capability.
- self-healing.

Replicas
- Aurora replicas up to 15 - failover to these is auto, not the same with MySQL read reps
- up to 5 MySQL read replicas

Since 2016 supports cross-region replication:
https://aws.amazon.com/about-aws/whats-new/2016/06/amazon-aurora-now-supports-cross-region-replication/
