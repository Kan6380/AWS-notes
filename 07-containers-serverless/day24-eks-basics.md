# Day 24 — EKS Fundamentals (Kubernetes on AWS)

## What it is
**EKS (Elastic Kubernetes Service)** is AWS's managed Kubernetes offering — AWS runs and manages the Kubernetes **control plane** (API server, etcd, scheduler) for you, while you manage the worker nodes (or use Fargate for fully serverless nodes too).

## Why it matters
Since your homelab already runs a self-managed 3-node kubeadm cluster, EKS is the natural "what would this look like in production on AWS" comparison — a strong, genuine talking point for interviews.

## Key concepts

### What AWS manages vs what you manage

| | Self-managed K8s (your homelab) | EKS |
|---|---|---|
| Control plane (API server, etcd, scheduler) | You manage (kubeadm) | AWS manages, highly available across AZs |
| Worker nodes | You manage (your 3 VMs) | You manage (EC2) OR fully serverless (Fargate) |
| Networking | Flannel CNI (your setup) | Amazon VPC CNI (pods get real VPC IPs) |
| Upgrades | Manual (`kubeadm upgrade`) | AWS-assisted, still requires your coordination |

### Core EKS-specific concepts
- **EKS Cluster** — the managed control plane
- **Node Group** — a group of EC2 instances (managed or self-managed) that join as worker nodes
- **Fargate Profile** — run pods on Fargate instead of EC2 nodes (no node management at all)
- **VPC CNI** — EKS's default networking plugin, gives each Pod a real IP address from the VPC's CIDR range (different from your Flannel overlay network approach)

## Hands-on / commands

```bash
# Create an EKS cluster (using eksctl, the common CLI tool for this)
eksctl create cluster --name my-cluster --region eu-west-1 --nodes 3

# Update kubeconfig to point kubectl at the EKS cluster
aws eks update-kubeconfig --name my-cluster --region eu-west-1

# Verify connection
kubectl get nodes
```

## Common exam gotchas
- EKS control plane is **highly available by design** — spans multiple AZs automatically, unlike a typical single-master kubeadm setup (worth noting as a real gap vs. your homelab's single master node)
- You still pay an hourly fee for the EKS control plane itself, separate from worker node costs
- "Managed Kubernetes with less operational overhead than self-hosting" → EKS is the answer

## My notes / things that confused me
_(fill in as you go — genuinely worth documenting the side-by-side differences from your own kubeadm cluster here, since you have real hands-on comparison material most candidates don't)_
