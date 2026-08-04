# Day 16 — IAM Roles vs Users (Deep Dive)

## What it is
A closer look at why IAM Roles are generally preferred over IAM Users for most access patterns, especially for applications and automation.

## Why it matters
This distinction comes up constantly in real jobs and interviews — knowing exactly when to use which, and why, signals strong practical understanding.

## Key concepts

| | User | Role |
|---|---|---|
| **Who/what uses it** | A specific person or app | Anyone/anything allowed to "assume" it |
| **Credentials** | Long-lived (password, access keys) | Temporary, auto-expiring (via STS) |
| **Best for** | Individual human console/CLI access (sparingly) | Services (EC2, Lambda), cross-account access, federation |
| **Security risk** | Higher — long-lived keys can leak | Lower — credentials expire automatically |

### Why Roles are preferred
1. No long-lived secret to leak, rotate, or forget about
2. Automatically scoped to a session — expires without manual cleanup
3. Same role can be assumed by many different trusted identities (users, services, federated logins) without duplicating permissions setup

### Real-world pattern: EC2 assuming a Role
Instead of hardcoding AWS credentials into an app running on EC2, you attach an **Instance Profile** (a role) to the EC2 instance. The app then automatically gets temporary credentials via the instance metadata service — no keys stored anywhere on disk.

## Hands-on / commands

```bash
# Create a role with a trust policy allowing EC2 to assume it
aws iam create-role --role-name ec2-s3-access \
  --assume-role-policy-document file://ec2-trust-policy.json

# Attach a permissions policy to that role
aws iam attach-role-policy --role-name ec2-s3-access \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Create an instance profile and add the role to it
aws iam create-instance-profile --instance-profile-name ec2-s3-profile
aws iam add-role-to-instance-profile \
  --instance-profile-name ec2-s3-profile --role-name ec2-s3-access

# Attach the instance profile to a running EC2 instance
aws ec2 associate-iam-instance-profile \
  --instance-id i-xxxxxxxx \
  --iam-instance-profile Name=ec2-s3-profile
```

## Common exam gotchas
- "An application on EC2 needs to access S3 — what's the best practice?" → attach an IAM Role via Instance Profile, never hardcode access keys
- Cross-account access always uses Roles, never Users — you can't directly share a User across AWS accounts
- `AssumeRole` API call is central to how roles actually get "activated" — worth remembering the name for exam questions

## My notes / things that confused me
_(fill in as you go — this maps closely to Azure AD PIM concepts you already know: temporary elevated access instead of standing permissions)_
