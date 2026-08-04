# Day 25 — ALB, Auto Scaling Groups, Launch Templates

## What it is
Three components that together create a scalable, self-healing, load-balanced application tier on EC2.

- **ALB (Application Load Balancer)** — distributes incoming traffic across multiple targets
- **Auto Scaling Group (ASG)** — automatically adds/removes EC2 instances based on demand or health
- **Launch Template** — defines what a new instance looks like when ASG creates one

## Why it matters
This trio is the classic AWS pattern for running a resilient, scalable web application — conceptually similar to what a Kubernetes Deployment + Service + HPA (Horizontal Pod Autoscaler) achieves, just at the EC2/VM level instead of the Pod level

## Key concepts

### Application Load Balancer (ALB)
- Operates at Layer 7 (HTTP/HTTPS) — can route based on path, host header, etc.
- Distributes traffic across multiple healthy targets (EC2 instances, containers, Lambda)
- Performs health checks — automatically stops sending traffic to unhealthy targets

### Launch Template
- Blueprint for new EC2 instances: AMI, instance type, key pair, security groups, user data script
- Used by the ASG every time it needs to launch a new instance

### Auto Scaling Group
- Maintains a desired number of healthy instances
- Scales based on:
  - **Target tracking** (e.g., keep average CPU at 50%)
  - **Step scaling** (add/remove based on CloudWatch alarm thresholds)
  - **Scheduled scaling** (predictable traffic patterns, e.g., business hours)
- Automatically replaces unhealthy instances (self-healing)

### How they fit together
```
Users → ALB → Target Group → EC2 instances (managed by ASG, launched from Launch Template)
```

## Hands-on / commands

```bash
# Create a launch template
aws ec2 create-launch-template \
  --launch-template-name my-template \
  --launch-template-data '{"ImageId":"ami-xxxxxxxx","InstanceType":"t2.micro"}'

# Create an Auto Scaling Group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-asg \
  --launch-template LaunchTemplateName=my-template \
  --min-size 2 --max-size 5 --desired-capacity 2 \
  --vpc-zone-identifier "subnet-xxxx,subnet-yyyy"

# Create an ALB
aws elbv2 create-load-balancer \
  --name my-alb \
  --subnets subnet-xxxx subnet-yyyy \
  --security-groups sg-xxxxxxxx
```

## Common exam gotchas
- ALB = Layer 7 (HTTP-aware routing); NLB (Network Load Balancer) = Layer 4 (raw TCP, ultra-low latency) — a common exam distinction
- ASG spans multiple AZs for high availability — always specify subnets across multiple AZs
- Scaling policies react to CloudWatch metrics — ASG itself doesn't "decide" to scale without a metric/alarm tied to it

## My notes / things that confused me

good comparison point if you've worked with Kubernetes rollouts or HPA before
