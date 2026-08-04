# Day 11 — EBS vs EFS vs S3 (Storage Comparison)

## What it is
Three different AWS storage types, each suited to different use cases. A very common exam and interview topic.

## Why it matters
Picking the wrong storage type for a workload is a classic real-world (and exam) mistake — e.g., trying to mount S3 like a hard drive, or using EBS when you need shared access across many instances.

## Key concepts

| | EBS | EFS | S3 |
|---|---|---|---|
| **Type** | Block storage | File storage | Object storage |
| **Attach to** | One EC2 instance at a time (per AZ) | Many EC2 instances simultaneously | Accessed via API, not "mounted" like a disk |
| **Scope** | Single Availability Zone | Multi-AZ (regional) | Regional, globally unique names |
| **Use case** | Boot volumes, databases needing low-latency block storage | Shared file systems (e.g., multiple web servers sharing content) | Backups, static websites, data lakes, large-scale unstructured data |
| **Scaling** | Manually resize | Auto-scales (pay for what you use) | Effectively unlimited |
| **Analogy** | A hard drive plugged into one computer | A shared network drive | A giant online filing cabinet |

## Hands-on / commands

```bash
# Create an EBS volume
aws ec2 create-volume --size 10 --availability-zone eu-west-1a --volume-type gp3

# Attach it to an instance
aws ec2 attach-volume --volume-id vol-xxxxxxxx --instance-id i-xxxxxxxx --device /dev/sdf

# Create an EFS file system
aws efs create-file-system --creation-token my-efs --performance-mode generalPurpose
```

## Common exam gotchas
- EBS volumes are tied to a **single AZ** — if the instance moves to another AZ, you can't attach the same EBS volume without a snapshot + new volume in that AZ
- EFS is the answer whenever a question says "shared file system across multiple EC2 instances"
- S3 is NOT a file system — you can't mount and randomly write/append to files the way you can on EBS/EFS; every S3 "write" replaces the whole object

## My notes / things that confused me
_(fill in as you go)_
