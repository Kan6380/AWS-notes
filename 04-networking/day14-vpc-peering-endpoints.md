# Day 14 — VPC Peering, VPC Endpoints

## What it is
- **VPC Peering** — a private network connection between two VPCs, letting resources in each communicate as if they were on the same network
- **VPC Endpoint** — a private connection from your VPC directly to an AWS service (like S3 or DynamoDB), without going over the public internet

## Why it matters
Both reduce reliance on the public internet for traffic that should stay private — improving security and often reducing data transfer costs.

## Key concepts

### VPC Peering
- Connects two VPCs (same or different AWS accounts, same or different regions)
- **Not transitive** — if VPC A is peered with B, and B is peered with C, A cannot automatically reach C. Each pair needs its own peering connection.
- Requires route table updates on both sides

### VPC Endpoints — two types
| Type | Used for | How it works |
|---|---|---|
| **Gateway Endpoint** | S3, DynamoDB only | Added as a route table entry, no additional cost |
| **Interface Endpoint** | Most other AWS services | Creates an ENI (Elastic Network Interface) with a private IP in your subnet, small hourly cost |

## Hands-on / commands

```bash
# Create a VPC peering connection
aws ec2 create-vpc-peering-connection --vpc-id vpc-A --peer-vpc-id vpc-B

# Accept the peering connection (from the accepter side)
aws ec2 accept-vpc-peering-connection --vpc-peering-connection-id pcx-xxxxxxxx

# Create a Gateway Endpoint for S3
aws ec2 create-vpc-endpoint \
  --vpc-id vpc-xxxxxxxx \
  --service-name com.amazonaws.eu-west-1.s3 \
  --route-table-ids rtb-xxxxxxxx
```

## Common exam gotchas
- "Not transitive" is a classic exam trap — expect a question testing whether you know A↔B and B↔C peering does NOT give A↔C connectivity
- CIDR blocks of peered VPCs **cannot overlap** — this is a common design mistake to catch early
- Gateway Endpoints (S3/DynamoDB) are free; Interface Endpoints (most other services) have an hourly + data cost — relevant for the cost-optimization exam domain

## My notes / things that confused me
_(fill in as you go)_
