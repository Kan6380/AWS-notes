# Day 21 — Database Backup & Snapshot Strategies

## What it is
Approaches to protecting database data against loss — automated backups, manual snapshots, and point-in-time recovery.

## Why it matters
Data loss scenarios (accidental deletion, corruption, ransomware) are a real risk — and a heavily tested exam theme under both the Resilient Architectures and Security domains.

## Key concepts

### RDS Automated Backups
- Enabled by default, daily backups + transaction logs
- Enables **Point-in-Time Recovery (PITR)** — restore to any second within your retention window (up to 35 days)
- Backups stored in S3 automatically, no manual action needed

### RDS Manual Snapshots
- User-initiated, kept until you explicitly delete them (don't expire automatically)
- Useful before risky changes (schema migration, major version upgrade)

### DynamoDB Backups
- **On-Demand backups** — manual, full backups, kept until deleted
- **Point-in-Time Recovery (PITR)** — continuous backups, restore to any point in the last 35 days (optional feature, must be enabled)

### EBS Snapshots
- Point-in-time copies of an EBS volume, stored in S3
- Incremental — only changed blocks since the last snapshot are stored (saves cost)

## Hands-on / commands

```bash
# Create a manual RDS snapshot
aws rds create-db-snapshot \
  --db-instance-identifier my-db \
  --db-snapshot-identifier my-db-snapshot-2026

# Restore RDS to point-in-time
aws rds restore-db-instance-to-point-in-time \
  --source-db-instance-identifier my-db \
  --target-db-instance-identifier my-db-restored \
  --restore-time 2026-08-01T12:00:00Z

# Create an EBS snapshot
aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "backup before upgrade"

# Enable DynamoDB PITR
aws dynamodb update-continuous-backups \
  --table-name Tasks \
  --point-in-time-recovery-specification PointInTimeRecoveryEnabled=true
```

## Common exam gotchas
- Automated backups are deleted if you delete the RDS instance — unless you explicitly take a final manual snapshot first
- PITR granularity is down to the second, not just daily snapshots — important distinction in scenario questions
- EBS snapshots are incremental, but each snapshot is still a complete, independently restorable point-in-time copy (you don't need the earlier snapshots to restore a later one)

## My notes / things that confused me
_(fill in as you go)_
