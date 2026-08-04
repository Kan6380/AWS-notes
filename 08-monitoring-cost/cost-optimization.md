# Day 27 — Cost Optimization — Savings Plans, Reserved vs Spot

## What it is
AWS offers several pricing models beyond standard On-Demand pricing, designed to reduce cost when you can commit to usage or tolerate interruption.

## Why it matters
This is the Cost-Optimized Architectures exam domain (20% of SAA-C03) — and directly useful knowledge for any real job where cloud spend matters.

## Key concepts

### EC2 Pricing Models

| Model | How it works | Best for |
|---|---|---|
| **On-Demand** | Pay per hour/second, no commitment | Unpredictable, short-term, or spiky workloads |
| **Reserved Instances (RI)** | Commit to 1 or 3 years for a specific instance type/region, up to ~72% discount | Steady, predictable, long-term workloads |
| **Savings Plans** | Commit to a $/hour spend for 1 or 3 years, more flexible than RIs (works across instance families/regions) | Predictable spend, but wanting flexibility — **AWS now generally recommends this over RIs** |
| **Spot Instances** | Bid on unused EC2 capacity, up to ~90% discount, can be interrupted with 2-min warning | Fault-tolerant, flexible workloads (batch jobs, CI/CD runners, big data processing) |

### S3 Cost Optimization
- Choose the right storage class (see Day 10 notes)
- Use lifecycle policies to auto-transition aging data
- Use S3 Intelligent-Tiering when access patterns are unpredictable

### General cost tools
- **AWS Budgets** — spend alerts
- **Cost Explorer** — visualize and analyze spending trends
- **Trusted Advisor** — automated recommendations (idle resources, underutilized instances, etc.)
- **Compute Optimizer** — recommends right-sizing for EC2/EBS/Lambda based on actual usage

## Hands-on / commands

```bash
# View current month cost by service (Cost Explorer via CLI)
aws ce get-cost-and-usage \
  --time-period Start=2026-08-01,End=2026-08-31 \
  --granularity MONTHLY \
  --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE

# Request a Spot Instance
aws ec2 request-spot-instances \
  --spot-price "0.03" \
  --instance-count 1 \
  --type "one-time" \
  --launch-specification file://spot-spec.json
```

## Common exam gotchas
- Spot Instances can be terminated by AWS with only a 2-minute warning — never use for stateful, critical, or time-sensitive workloads without fault tolerance built in
- Savings Plans are more flexible than Reserved Instances (can apply across instance families) — current AWS guidance favors Savings Plans for most use cases
- Reserved Instances are billing constructs, not physical reservations — they don't guarantee capacity by themselves (unless combined with Capacity Reservations)

## My notes / things that confused me
_(fill in as you go)_
