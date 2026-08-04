# Day 23 — ECS & Fargate Fundamentals

## What it is
**ECS (Elastic Container Service)** is AWS's own container orchestration service — runs and manages Docker containers. **Fargate** is a serverless compute option for ECS, meaning you don't manage the underlying EC2 instances at all.

## Why it matters
Given your homelab already runs Kubernetes (self-managed), understanding ECS/Fargate gives you the AWS-native alternative — useful context for interviews comparing "why Kubernetes vs ECS."

## Key concepts

### ECS Launch Types

| | EC2 Launch Type | Fargate Launch Type |
|---|---|---|
| **Who manages servers** | You (provision & manage EC2 instances) | AWS (fully serverless) |
| **Control** | More control over instance type/config | Less control, less operational overhead |
| **Billing** | Per EC2 instance-hour | Per task's vCPU/memory usage |
| **Best for** | Cost optimization at scale, custom AMI needs | Simplicity, variable workloads, no infra management |

### Core ECS concepts
- **Task Definition** — blueprint describing your container(s): image, CPU/memory, ports, environment variables (similar in spirit to a Kubernetes Pod spec)
- **Task** — a running instance of a Task Definition (like a Pod)
- **Service** — ensures a specified number of Tasks are always running, handles load balancer registration (like a Kubernetes Deployment + Service combined)
- **Cluster** — logical grouping of Tasks/Services (like a Kubernetes cluster, but AWS-managed)

### ECS vs EKS vs Kubernetes (self-managed)
| | ECS | EKS | Self-managed K8s (your homelab) |
|---|---|---|---|
| Orchestrator | AWS proprietary | Kubernetes | Kubernetes |
| Control plane management | Fully AWS-managed | AWS-managed | You manage it |
| Portability | AWS-only | Portable (K8s standard) | Fully portable |
| Learning curve | Simpler | Steeper (K8s concepts) | Steepest (+ infra) |

## Hands-on / commands

```bash
# Create an ECS cluster
aws ecs create-cluster --cluster-name my-cluster

# Register a task definition (from a JSON file)
aws ecs register-task-definition --cli-input-json file://task-def.json

# Run a service using Fargate
aws ecs create-service \
  --cluster my-cluster \
  --service-name my-service \
  --task-definition my-task \
  --desired-count 2 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xxxx],securityGroups=[sg-xxxx],assignPublicIp=ENABLED}"
```

## Common exam gotchas
- Fargate = no EC2 instances to patch/manage; EC2 launch type = you control the underlying instances (useful for GPU workloads, custom AMIs, cost optimization at scale)
- ECS Service vs Task — a Service maintains desired Task count automatically (self-healing), a standalone Task does not
- "Run containers without managing servers" → Fargate is almost always the intended answer

## My notes / things that confused me
_(fill in as you go — worth comparing directly against your own kubeadm/Flannel homelab setup for a strong interview talking point)_
