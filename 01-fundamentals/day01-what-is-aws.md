# Day 1 — What is AWS? Cloud Computing Basics, Regions & Availability Zones

## What it is
AWS (Amazon Web Services) is a cloud computing platform that lets you rent computing resources — servers, storage, databases, networking — over the internet, instead of buying and maintaining your own physical hardware.

"Cloud computing" simply means using someone else's data centers (accessed remotely) instead of running your own.

## Why it matters
Before cloud computing, companies had to buy physical servers, house them in a data center, maintain the hardware, and plan capacity years in advance. If you guessed wrong on how much hardware you needed, you either wasted money (over-provisioned) or crashed under load (under-provisioned).

AWS removes that problem: you can spin up a server in minutes, scale it up or down on demand, and pay only for what you use.

## Key concepts

- **On-demand resources** — provision compute, storage, or database capacity instantly, no upfront hardware purchase
- **Pay-as-you-go pricing** — pay for what you use, not fixed capacity
- **Elasticity** — scale resources up or down automatically based on demand
- **Global infrastructure** — AWS data centers exist worldwide, so you can run your app close to your users

### Regions
A **Region** is a physical geographic location where AWS has clusters of data centers (e.g., `eu-west-1` = Ireland, `us-east-1` = North Virginia). Each region is fully independent — data doesn't automatically leave a region unless you configure it to.

### Availability Zones (AZs)
Each Region is made up of multiple **Availability Zones** — physically separate data centers within that region, each with independent power, cooling, and networking. Most regions have 3+ AZs.

**Why AZs matter:** if you deploy your app across multiple AZs, and one AZ has an outage (power failure, fire, flood), your app keeps running because the other AZs are unaffected. This is the foundation of AWS's high-availability design.

```
Region: eu-west-1 (Ireland)
├── Availability Zone A
├── Availability Zone B
└── Availability Zone C
```

## Hands-on / commands
No commands yet — this is a conceptual day. Just explore:
1. Log into the AWS Console
2. Top-right corner → check which Region you're currently in
3. Switch regions and notice how resources (EC2 instances, S3 buckets you've made) disappear — because most resources are region-scoped

## Common exam gotchas
- **IAM is global**, not region-scoped (comes up later — don't confuse this with EC2/S3 which ARE region-scoped)
- Choosing a region isn't just about proximity to users — also affects **pricing** and **service availability** (not every service is available in every region)
- "High availability" = across multiple AZs. "Disaster recovery" = across multiple Regions (bigger blast radius protection)

## My notes / things that confused me
_(fill in as you go)_
