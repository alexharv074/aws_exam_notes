# EC2

Pricing models:

- On demand: fixed rate by the hour.
- Reserved Instances: discounted hourly rate with a commitment to run for 1 or 3 years.
- Spot: bid on whatever price you like if your app can handle flexible start/end times.
- Dedicated hosts: Physical EC2 instances dedicated to your use. Dedicated hosts useful if say a country or license doesn't allow multi-tenant. Up to 70% off on-demand.

Spot instances:
- don't pay at all if Amazon terminates the instance.
- pay for the full hour if you terminate the instance.

Reserved Instances:
- Stardard RIs: ~ 75% less than on-demand.
- Convertible RIs: ~ 54% less than on-demand - can change attributes as long as changing to a more expensive RI.
- Scheduled RIs: e.g. launch only during business hours.

# EBS

Volumes are auto-replicated to protect from failure of a single component.

Types:

GP2
- balances price and performance.
- 3 IOPS per GB up to 10000 IOPS and burst to +3000 IOPS.

Provisioned IOPS
- for IO intensive e.g. NoSQL > 10000 IOPS.
- 20000 IOPS per volume.

Volumes exist on EBS / Snapshots on S3.

Snapshots:
- incremental

Root FS EBS Snapshots:
- stop the instance before taking the snapshot.
- take a snap while the instance is running.
- create AMIs from EBS-backed instances AND snapshots.
- can change the EBS size & type on the fly.
- volumes in the same AZ as the EC2 instance.
- snapshots of encrypted volumes are also encrypted.

Throughput Optimized HDD (st1)
- Designed for high-throughput MapReduce, Kafka, ETL, log processing, and data warehouse workloads. Cannot be a boot volume.

Cold HDD (sc1)
- Designed for workloads similar to those for Throughput Optimized HDD that are accessed less frequently. Also cannot be a boot volume.

Magnetic (standard)
- Lowest cost per GB.
- Ideal for infrequent access.

Encryption at rest:

> In order to enable encryption at rest using EC2 and EBS, you must configure encryption when creating the EBS volume.

It uses a Customer Master Key (CMKs).

# Security Group

An SG is a virtual firewall.

- All inbound blocked by default, but all outbound allowed by default.
- Changes take effect immediately.
- Many EC2 instances per SG ; many SGs per EC2.
- SGs are STATEFUL.


