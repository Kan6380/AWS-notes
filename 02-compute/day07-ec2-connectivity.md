# Day 7 — Elastic IPs, Key Pairs, Connecting via SSH

## What it is
Covers the pieces that let you reliably reach and authenticate into an EC2 instance.

## Why it matters
Instances get new public IPs every time they stop/start by default — which breaks anything relying on a fixed address (DNS records, scripts, bookmarks). Elastic IPs solve that.

## Key concepts

### Elastic IP (EIP)
A static, public IPv4 address you can allocate to your AWS account and attach to an instance. Unlike the default public IP (which changes on stop/start), an EIP stays fixed until you release it.

⚠️ **Cost gotcha**: an EIP is free **only while attached to a running instance**. An unattached (or attached-to-stopped-instance) EIP incurs hourly charges — a very common "why am I being charged for nothing" surprise.

### Key Pairs
Asymmetric key pair (public/private) used for SSH authentication instead of passwords.
- AWS stores the **public key** on the instance
- You keep the **private key** (`.pem` file) — never shared, never uploaded anywhere

### Connecting via SSH
```bash
chmod 400 my-key.pem
ssh -i my-key.pem ec2-user@<public-ip-or-EIP>
```

Default usernames vary by AMI:
| AMI | Default user |
|---|---|
| Amazon Linux | `ec2-user` |
| Ubuntu | `ubuntu` |
| RHEL | `ec2-user` or `root` |

## Hands-on / commands

```bash
# Allocate an Elastic IP
aws ec2 allocate-address

# Associate it with an instance
aws ec2 associate-address --instance-id i-xxxxxxxxxxxxx --allocation-id eipalloc-xxxxxxxx

# Release it when no longer needed (avoid charges)
aws ec2 release-address --allocation-id eipalloc-xxxxxxxx
```

## Common exam gotchas
- Unattached Elastic IPs cost money — always release ones you're not using
- Losing your `.pem` private key means you cannot recover SSH access the normal way — you'd need to detach the EBS volume, attach to another instance, add a new key, and reattach (advanced recovery process)
- EIPs are tied to a Region — can't move one across regions directly

## My notes / things that confused me

Elastic IP charges

Elastic IP — I thought it was just a free static IP

I assumed once AWS gives you an Elastic IP, it's yours and free, since it's "just an IP address." Turns out:

An EIP is free only while it's attached to a running instance
The moment you stop the instance, or don't attach the EIP to anything at all, AWS starts charging for it hourly

This actually makes sense once I thought about it — AWS has a limited pool of public IPv4 addresses, so they charge for ones sitting idle to discourage people from hoarding addresses they're not using. Still caught me off guard the first time.
