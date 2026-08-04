# Day 29 — CI/CD on AWS — CodePipeline Overview + GitHub Actions + OIDC

## What it is
Two approaches to CI/CD when working with AWS: AWS's own native CI/CD services, or an external tool (like GitHub Actions, which you already use) authenticating securely to AWS.

## Why it matters
ties together a typical GitHub Actions pipeline (test → build-and-push → deploy) with how it would securely interact with AWS resources

## Key concepts

### AWS-native CI/CD services (high level)
| Service | Purpose |
|---|---|
| **CodeCommit** | Git repository hosting (AWS's own, less commonly used now that GitHub dominates) |
| **CodeBuild** | Compiles/builds/tests code |
| **CodeDeploy** | Automates deployment to EC2, ECS, Lambda |
| **CodePipeline** | Orchestrates the full pipeline — connects source → build → deploy stages |

### Using GitHub Actions with AWS (your actual likely path)
Instead of adopting AWS-native CI/CD tools, you can keep using GitHub Actions and just have it securely call AWS APIs — for example, to push a Docker image to ECR, or deploy to EKS/ECS.

**Best practice: OIDC instead of static access keys**
Rather than storing `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` as GitHub Secrets (long-lived, risky if leaked), configure an OIDC trust relationship so GitHub Actions can request short-lived AWS credentials directly.

Example GitHub Actions step using OIDC:
```yaml
permissions:
  id-token: write   # required for OIDC
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy-role
          aws-region: eu-west-1

      - name: Push to ECR
        run: |
          aws ecr get-login-password | docker login --username AWS --password-stdin <account>.dkr.ecr.eu-west-1.amazonaws.com
          docker push <account>.dkr.ecr.eu-west-1.amazonaws.com/my-app:latest
```

## Hands-on / commands

```bash
# Create an ECR repository to push images to
aws ecr create-repository --repository-name my-app

# Authenticate Docker to ECR
aws ecr get-login-password --region eu-west-1 | \
  docker login --username AWS --password-stdin <account-id>.dkr.ecr.eu-west-1.amazonaws.com

# Push an image
docker tag my-app:latest <account-id>.dkr.ecr.eu-west-1.amazonaws.com/my-app:latest
docker push <account-id>.dkr.ecr.eu-west-1.amazonaws.com/my-app:latest
```

## Common exam gotchas
- CodePipeline orchestrates stages but doesn't do the actual build/deploy work itself — it calls CodeBuild/CodeDeploy (or third-party tools) to do that
- OIDC-based GitHub Actions auth is a modern best practice — reduces risk vs. storing static AWS keys as secrets
- ECR (Elastic Container Registry) is AWS's Docker registry — equivalent role to your current Docker Hub usage, just AWS-native

## My notes / things that confused me
natural next step for anyone running GitHub Actions who wants to deploy to AWS instead of a self-hosted environment
