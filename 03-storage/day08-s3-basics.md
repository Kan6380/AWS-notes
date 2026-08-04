# Day 8 — What is S3? Buckets vs Objects

## What it is
**S3 (Simple Storage Service)** is AWS's object storage service — store virtually unlimited files ("objects") in containers called "buckets," accessed over the internet via API/CLI/Console.

## Why it matters
S3 is one of the most heavily used AWS services — backups, static websites, data lakes, app file storage, logs. It's foundational to almost every AWS architecture.

## Key concepts

### Bucket vs Object
- **Bucket** = the container (like a top-level folder). Name must be **globally unique** across ALL AWS accounts. Tied to a specific Region.
- **Object** = the actual file stored inside a bucket, identified by a unique **key** (not a real file path — S3 is a flat namespace; folders are simulated using `/` in the key name)

### Key facts
- Object size: 0 bytes up to 5TB
- Single PUT upload capped at 5GB — anything larger needs **multipart upload**
- Durability: 99.999999999% (11 nines) — data replicated across multiple facilities automatically
- **Block Public Access** is ON by default for all new buckets — a safety default

## Hands-on / commands

```bash
# Create a bucket
aws s3 mb s3://your-unique-bucket-name-2026 --region eu-west-1

# Upload a file
aws s3 cp myfile.txt s3://your-unique-bucket-name-2026/

# List contents
aws s3 ls s3://your-unique-bucket-name-2026/

# Download a file
aws s3 cp s3://your-unique-bucket-name-2026/myfile.txt ./downloaded.txt

# Delete a bucket (must be empty first)
aws s3 rb s3://your-unique-bucket-name-2026
```

## Common exam gotchas
- **403 Forbidden** = permission problem (missing bucket policy, Block Public Access still on)
- **404 Not Found** = the object genuinely doesn't exist (wrong key, not uploaded, wrong index document)
- Bucket names must be lowercase, no underscores, DNS-compliant

## My notes / things that confused me
_(fill in as you go)_
