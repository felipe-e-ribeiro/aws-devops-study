# Amazon RDS

[[Amazon RDS]] is a relational database service managed by AWS.

It supports the following database engines:
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server
- Amazon Aurora

You can use up to 15 read replicas. All read replicas are asynchronous, which means they are "eventually consistent."

Read replicas deployed within the same region are not charged for data transfer because it is a fully managed service by AWS. **IMPORTANT:** Cross-region read replicas do incur data transfer costs.

This differs from a Multi-AZ deployment, which is used for disaster recovery.
In a Multi-AZ deployment, replication is synchronous. However, by default, the standby instance cannot be used for read or write operations until the cluster is promoted to primary.

There is no downtime required to enable Multi-AZ configuration.

[[Amazon RDS Proxy]] can be used to improve database scalability by supporting connection pooling.
