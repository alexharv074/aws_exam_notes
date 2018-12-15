# Elastic Beanstalk

- High-level structure is an "Application"
- Your application can be one or more EB applications.
- Applications can have multiple environments (prod, dev etc).
- Environments are either single-instance or scalable.
- Versions are packages that represent the version i.e. zip files.
- Application can be split in tiers (web tier, app tier etc)
- EB is free but you pay for the resources.
- If the app creates the RDS, it is deleted again when the app is deleted.

Know all the languages.

- PHP, Python, Node.js, Ruby, .NET, Java, Go.

## Deployment policies

Deployment policy – Choose from the following deployment options:

- All at once – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs.

- Rolling – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment's capacity by the number of instances in a batch.

- Rolling with additional batch – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process.

- Immutable – Deploy the new version to a fresh group of instances by performing an immutable update.

Blue/green deployments require that your environment runs independently of your production database, if your application uses one. If your environment has an Amazon RDS DB instance attached to it, the data will not transfer over to your second environment, and will be lost if you terminate the original environment.

## Configuring

Change instance type on a running config. Create a config file in S3 with instance type use the same during instance creation. Confusing! Is this right? But you can't use the configuration details page to change a running config.

## Custom platform

You create your own Elastic Beanstalk platform using Packer, which is an open-source tool for creating machine images for many platforms, including AMIs for use with Amazon EC2.
