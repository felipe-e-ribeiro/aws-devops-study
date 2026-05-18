# Amazon ElastiCache

[[Amazon ElastiCache]] is a managed in-memory cache service provided by AWS.

It supports the following caching engines:
- Redis
- Memcached

You can use up to 5 read replicas. All read replicas are asynchronous, meaning they are "eventually consistent."

### Redis
- Supports Multi-AZ with Auto-failover.
- Supports backup and restore operations.
- Supports Sets and Sorted Sets data structures.
- Has read replicas for scalability.

### Memcached
- Does NOT support Multi-AZ with Auto-failover.
- Does NOT support backups.
- Does NOT provide data persistence.

There are two deployment architectures for Redis:

#### Cluster Mode Disabled
- One primary node and up to 5 read replicas.
- Asynchronous replication.
- A single shard.
- All replicas contain the entire dataset.
- Multi-AZ enabled for failover.
- Scaling methods:
  - **Horizontal:** Scale out/in by adding or removing read replicas (maximum of 5).
  - **Vertical:** Scale up/down by changing the instance type. Behind the scenes, a new NodeGroup is created, and once ready, DNS is updated to point to the new NodeGroup.

#### Cluster Mode Enabled
- Data is distributed across multiple shards, improving write scalability.
- Each shard can have up to 5 read replicas.
- Multi-AZ enabled for failover.
- You can have up to 500 nodes per cluster:
  - 500 shards with a single master each.
  - 250 shards with 1 master and 1 replica each.
  - ... up to 83 shards with 1 master and 5 replicas each.

### Redis Connection Endpoints

- **Standalone:**
  - A single endpoint for both read and write operations.
- **Cluster Mode Disabled:**
  - **Primary Endpoint:** Used for all write operations.
  - **Reader Endpoint:** Used to distribute read operations.
  - **Node Endpoint:** Used for reading from a specific node.
- **Cluster Mode Enabled:**
  - **Configuration Endpoint:** Used for all read/write operations (supports Cluster Mode Enabled commands).
  - **Node Endpoint:** Used for reading from a specific node.
