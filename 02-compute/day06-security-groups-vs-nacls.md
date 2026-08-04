# Day 6 — Security Groups vs NACLs

## What it is
Both are firewalls that control traffic in AWS — but they operate at different levels and behave differently.

- **Security Group (SG)** — a virtual firewall attached to an instance (or ENI)
- **NACL (Network ACL)** — a virtual firewall attached to a subnet

## Why it matters
This is one of the most commonly confused (and exam-tested) topics in AWS networking. Getting the distinction solid early saves a lot of debugging pain later.

## Key concepts

| | Security Group | NACL |
|---|---|---|
| **Level** | Instance-level | Subnet-level |
| **Rule type** | Allow rules only | Allow AND Deny rules |
| **State** | Stateful (return traffic auto-allowed) | Stateless (return traffic must be explicitly allowed) |
| **Evaluation** | All rules evaluated, most permissive wins | Rules evaluated in order (lowest number first), first match wins |
| **Default behavior** | Denies all inbound by default, allows all outbound | Allows all traffic by default (default NACL) |

### Stateful vs Stateless — the part that trips people up
- **Security Groups are stateful**: if you allow inbound traffic on port 80, the response traffic is automatically allowed out — you don't need a separate outbound rule for it.
- **NACLs are stateless**: you must explicitly allow BOTH the inbound rule AND the corresponding outbound rule for return traffic, or the connection breaks.

## Hands-on / commands

```bash
# List security groups
aws ec2 describe-security-groups

# Add an inbound rule (allow SSH from your IP only)
aws ec2 authorize-security-group-ingress \
  --group-id sg-xxxxxxxx \
  --protocol tcp --port 22 \
  --cidr <your-ip>/32

# List NACLs
aws ec2 describe-network-acls
```

## Common exam gotchas
- Classic exam question: "Users can't connect to an EC2 instance — what could be wrong?" → check BOTH Security Group AND NACL, since either could be blocking
- NACL rule numbers matter — lower number evaluated first, and once a rule matches, evaluation stops (unlike SG which checks all rules)
- Security groups can only ALLOW — you cannot explicitly deny with an SG. To block specific traffic, use a NACL instead

## My notes / things that confused me
_(fill in as you go)_
