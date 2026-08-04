# Day 12 — What is a VPC? Subnets, Route Tables

## What it is
A **VPC (Virtual Private Cloud)** is your own isolated, private network inside AWS — where you launch resources like EC2 instances. Think of it as your own private data center network, fully software-defined.

## Why it matters
Every EC2 instance, RDS database, and most other AWS resources live inside a VPC. Understanding VPC structure is foundational to almost everything else in AWS networking.

## Key concepts

### CIDR Block
The IP address range for your VPC, e.g., `10.0.0.0/16` (65,536 addresses).

### Subnets
Subdivisions of a VPC's IP range, each tied to a **single Availability Zone**.
- **Public subnet** — has a route to an Internet Gateway (resources can be reached from the internet)
- **Private subnet** — no direct route to the internet (used for databases, backend services)

### Route Tables
Define where network traffic is directed. Each subnet is associated with a route table.

Example public subnet route table:
```
Destination      Target
10.0.0.0/16       local
0.0.0.0/0         igw-xxxxxxxx   (Internet Gateway)
```

## Hands-on / commands

```bash
# Create a VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create a subnet
aws ec2 create-subnet --vpc-id vpc-xxxxxxxx --cidr-block 10.0.1.0/24 --availability-zone eu-west-1a

# Create a route table and associate it
aws ec2 create-route-table --vpc-id vpc-xxxxxxxx
aws ec2 associate-route-table --route-table-id rtb-xxxxxxxx --subnet-id subnet-xxxxxxxx
```

## Common exam gotchas
- A subnet is "public" only because its route table points `0.0.0.0/0` to an Internet Gateway — nothing inherently makes a subnet public otherwise
- Subnets cannot span multiple AZs — one subnet = one AZ, always
- AWS reserves 5 IP addresses in every subnet (first 4 and last 1) — factor this into CIDR sizing

## My notes / things that confused me
_(fill in as you go — this diagram from Day 1's networking flow is worth revisiting here: Browser → DNS → LB → Ingress → Service → Pod, similar layered thinking applies to VPC traffic flow)_
