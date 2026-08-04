# Day 28 — CloudFormation Basics — Infrastructure as Code

## What it is
**CloudFormation** is AWS's native Infrastructure as Code (IaC) service — you describe your desired AWS resources in a YAML or JSON template, and CloudFormation creates, updates, or deletes them for you, tracking everything as a single **Stack**.

## Why it matters
Manually clicking through the Console to create resources isn't repeatable or version-controllable. IaC lets you treat infrastructure like code — reviewable, versioned in Git, and consistently reproducible. 
This is conceptually the AWS-native counterpart to Terraform — worth comparing the two if you're deciding which IaC tool to learn.

## Key concepts

### Template structure
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-cf-bucket-2026

Outputs:
  BucketName:
    Value: !Ref MyBucket
```

### Core concepts
- **Template** — the YAML/JSON file describing resources
- **Stack** — a deployed instance of a template (a set of actual AWS resources CloudFormation manages together)
- **Change Set** — a preview of what will change before you actually apply an update (safety check)
- **Drift Detection** — detects when someone manually changed a resource outside of CloudFormation, breaking the "source of truth"

## Hands-on / commands

```bash
# Create a stack from a template
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml

# Check stack status
aws cloudformation describe-stacks --stack-name my-stack

# Preview changes before applying (Change Set)
aws cloudformation create-change-set \
  --stack-name my-stack \
  --template-body file://updated-template.yaml \
  --change-set-name my-changes

# Delete a stack (removes all its resources)
aws cloudformation delete-stack --stack-name my-stack
```

## Common exam gotchas
- Deleting a Stack deletes all resources it created by default (unless a resource has a `DeletionPolicy: Retain`)
- CloudFormation is declarative — you describe the end state, not the steps to get there (same philosophy as Terraform/Kubernetes manifests you're already used to)
- Rollback on failure is automatic by default — if part of a stack update fails, CloudFormation rolls back to the last known good state

## My notes / things that confused me
good place to note direct comparisons once you start learning Terraform
