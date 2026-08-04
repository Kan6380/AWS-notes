# Day 19 — What is RDS? Multi-AZ vs Read Replicas

## What it is
**RDS (Relational Database Service)** is AWS's managed relational database service — supports MySQL, PostgreSQL, MariaDB, SQL Server, Oracle. AWS handles patching, backups, and infrastructure so you just use the database.

## Why it matters
Managing your own database server — patching, backups, failover, scaling — is time-consuming and error-prone, especially as an application grows. RDS automates that operational overhead entirely, letting you focus on your application instead of database maintenance. This is especially valuable for teams running production workloads where downtime or a missed backup could mean real consequences.

## Key concepts

### Multi-AZ
Creates a **synchronous standby replica** in a different Availability Zone. If the primary fails, AWS automatically fails over to the standby — used purely for **high availability/disaster recovery**, not for improving read performance.

### Read Replicas
Creates an **asynchronous copy** of your database, used to offload read traffic (reporting, analytics queries) from the primary. Can be in the same region or cross-region. Used for **scaling reads**, not for automatic failover (though a read replica CAN be manually promoted to a standalone primary).

### Comparison

| | Multi-AZ | Read Replica |
|---|---|---|
| **Purpose** | High availability / failover | Read scalability |
| **Replication** | Synchronous | Asynchronous |
| **Automatic failover** | Yes | No (manual promotion only) |
| **Can serve read traffic?** | No (standby is not directly queryable) | Yes |

## Hands-on / commands

```bash
# Create a Multi-AZ RDS instance
aws rds create-db-instance \
  --db-instance-identifier my-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password mypassword \
  --allocated-storage 20 \
  --multi-az

# Create a read replica from an existing instance
aws rds create-db-instance-read-replica \
  --db-instance-identifier my-db-replica \
  --source-db-instance-identifier my-db
```

## Common exam gotchas
- Multi-AZ standby is NOT readable — a common exam trap answer choice tries to make you think it improves read performance, it doesn't
- Read Replicas can be promoted to standalone instances, but this breaks replication permanently — it's a one-way operation
- "Reduce read load on primary database" → Read Replica. "Ensure automatic failover during an AZ outage" → Multi-AZ. These get mixed up constantly on the exam

## My notes / things that confused me
good place to note any real debugging experience you've had with database health checks or readiness probes
