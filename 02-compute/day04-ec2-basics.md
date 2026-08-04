# Day 4 — What is EC2? Instance Types, AMIs

## What it is
**EC2 (Elastic Compute Cloud)** is AWS's core virtual server service. It lets you rent a virtual machine ("instance") in the cloud, choosing the CPU, memory, storage, and OS you need.

## Why it matters
EC2 is the foundational compute service in AWS — most other compute services (ECS, EKS, even some Lambda under-the-hood infra) ultimately run on EC2 infrastructure. Understanding it deeply pays off across the whole platform.

## Key concepts

### Instance Types
Named like `t2.micro`, `m5.large`, `c5.xlarge` — the letter indicates the family (purpose), the number is generation, and the size (micro/small/large/xlarge) indicates CPU/RAM.

| Family | Purpose |
|---|---|
| **T** (t2, t3) | Burstable, general purpose, cheap — good for dev/test |
| **M** (m5, m6) | General purpose, balanced CPU/RAM |
| **C** (c5, c6) | Compute-optimized — CPU-heavy workloads |
| **R** (r5, r6) | Memory-optimized — RAM-heavy workloads (databases) |
| **I** (i3) | Storage-optimized — high-speed local disk |

### AMI (Amazon Machine Image)
A template that defines what's on the instance when it launches — OS, pre-installed software, configuration. You can:
- Use AWS-provided AMIs (Amazon Linux, Ubuntu, Windows Server)
- Use Marketplace AMIs (pre-configured software stacks)
- Create your own custom AMI from a configured instance (useful for repeatable deployments)

### Instance States
`pending → running → stopping → stopped → terminated`
- **Stopped** = instance off, but EBS storage persists, you're not charged for compute (still charged for storage)
- **Terminated** = instance permanently deleted

## Hands-on / commands

```bash
# List available AMIs (Amazon Linux example)
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm*"

# List your running instances
aws ec2 describe-instances

# Stop / start / terminate an instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
aws ec2 start-instances --instance-ids i-0123456789abcdef0
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

## Common exam gotchas
- **Stopped ≠ Terminated** — stopped instances still incur EBS storage charges; terminated instances (by default) delete their root EBS volume too
- Choosing the wrong instance family is a common exam trap — e.g., picking a compute-optimized instance for a memory-heavy database workload
- **Instance Store vs EBS** — Instance Store is physically attached, ephemeral (data lost on stop/terminate); EBS is network-attached, persistent

## My notes / things that confused me
Here's that written up for your "My notes / things that confused me" section in `02-compute/ec2-basics.md`:

---

## My notes / things that confused me

**Stopped vs. still being charged**

At first I assumed stopping an EC2 instance meant I wouldn't be charged at all — like turning off a light switch. Turns out that's only half true.

- **Compute charges stop** — you're no longer billed for the EC2 instance itself once it's stopped
- **Storage charges continue** — the EBS volume attached to that instance (your root volume + any additional volumes) keeps existing on disk, and AWS keeps billing for that storage even while the instance is off

So "stopped" really means *"the compute is off, but your disk is still sitting there costing money."* The only way to fully stop being charged is to either:
1. **Terminate** the instance (deletes it, and by default deletes the root EBS volume too — unless "Delete on Termination" is unchecked)
2. Or manually delete the EBS volume separately after stopping

**Why this actually matters**

This is a common real-world "surprise bill" trap — someone stops a bunch of instances thinking they've cut costs to zero, then finds unexpected charges on their statement weeks later because the storage never actually went anywhere. Worth remembering as a genuine gotcha, not just an exam fact.

---
