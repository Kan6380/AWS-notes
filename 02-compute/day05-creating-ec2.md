# Day 5 — Creating Your First EC2 Instance (Step by Step)

## What it is
A hands-on walkthrough of actually launching an EC2 instance via the Console, then connecting to it.

## Why it matters
Reading about EC2 is one thing — actually launching, connecting to, and terminating an instance builds real muscle memory and confirms your understanding of the pieces from Day 4.

## Key concepts (recap before doing)
- Instance type, AMI, Key Pair, Security Group, Storage — all chosen at launch time

## Hands-on / steps

### Via Console
1. EC2 Dashboard → **Launch Instance**
2. **Name** your instance (e.g., `my-first-ec2`)
3. **Choose AMI** → Amazon Linux 2023 (Free tier eligible)
4. **Instance type** → `t2.micro` (Free tier eligible)
5. **Key pair** → Create new key pair → download the `.pem` file (⚠️ you can't re-download this later — save it safely)
6. **Network settings** → Allow SSH traffic from "My IP" only (not 0.0.0.0/0 — security best practice)
7. **Storage** → leave default (8GB gp3)
8. Click **Launch Instance**

### Connect via SSH (Mac/Linux/WSL)
```bash
chmod 400 my-key.pem
ssh -i my-key.pem ec2-user@<public-ip>
```

### Connect via EC2 Instance Connect (browser-based, no key needed)
EC2 Console → select instance → **Connect** → **EC2 Instance Connect** tab → Connect

### Terminate when done (avoid charges)
```bash
aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxx
```
Or via Console: select instance → Instance State → Terminate

## Common exam gotchas
- Forgetting to restrict Security Group SSH access to your IP only is a common real-world security mistake (0.0.0.0/0 = open to the entire internet)
- If SSH connection times out, the most common causes are: Security Group not allowing port 22, or instance doesn't have a public IP
- `.pem` file permissions matter — SSH will refuse to use a key file with overly permissive permissions (`chmod 400` fixes this)

## My notes / things that confused me
_(fill in as you go — good place for a screenshot of your first successful SSH connection)_
