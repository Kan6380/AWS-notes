# Day 13 — Internet Gateway, NAT Gateway

## What it is
Two different components that let VPC resources reach the internet — but for very different purposes.

- **Internet Gateway (IGW)** — lets resources in a public subnet be reached FROM the internet (two-way)
- **NAT Gateway** — lets resources in a PRIVATE subnet reach OUT to the internet, without being reachable FROM the internet (one-way)

## Why it matters
This distinction is one of the most-tested VPC concepts on SAA-C03 — and matters practically any time you have a private backend (like a database) that still needs outbound internet access (e.g., to download OS updates or call an external API).

## Key concepts

### Internet Gateway
- Attached to the VPC (one per VPC)
- Enables both inbound and outbound internet traffic
- Used for public subnets — e.g., a web server that needs to be reached by users

### NAT Gateway
- Sits inside a **public subnet**
- Private subnet resources route outbound traffic through it
- Allows outbound-only internet access — internet cannot initiate connections back in
- **Costs money per hour + data processed** (unlike IGW which has no hourly charge)

### Typical architecture
```
Internet
   │
Internet Gateway
   │
Public Subnet (NAT Gateway lives here)
   │
Private Subnet (EC2/RDS route outbound traffic → NAT Gateway → IGW → Internet)
```

## Hands-on / commands

```bash
# Create and attach an Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway --vpc-id vpc-xxxxxxxx --internet-gateway-id igw-xxxxxxxx

# Create a NAT Gateway (needs an Elastic IP and must sit in a public subnet)
aws ec2 allocate-address
aws ec2 create-nat-gateway --subnet-id subnet-xxxxxxxx --allocation-id eipalloc-xxxxxxxx
```

## Common exam gotchas
- A private subnet's route table must point `0.0.0.0/0` to the **NAT Gateway**, not the IGW directly
- NAT Gateway is AZ-specific — for high availability, deploy one NAT Gateway per AZ (a single NAT Gateway is a single point of failure)
- NAT Instance (older, EC2-based alternative) vs NAT Gateway (managed, AWS-preferred) — NAT Gateway is almost always the better/tested answer now

## My notes / things that confused me

NAT Gateway isn't free like IGW

I assumed NAT Gateway was free, same as Internet Gateway

Internet Gateway has no hourly charge — so I assumed NAT Gateway worked the same way since they sound related. Actually NAT Gateway charges per hour it exists, plus per GB of data processed through it — can add up fast if you're not paying attention, especially if you spin one up per AZ for high availability (which is the recommended practice).
