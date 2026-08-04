# Day 15 — What is IAM? Users, Groups, Roles, Policies

## What it is
**IAM (Identity and Access Management)** controls who can do what in your AWS account — it's the security layer that every single AWS API call passes through.

## Why it matters
Nothing happens in AWS without IAM approving it first. It's arguably the most heavily tested topic on SAA-C03 (Security domain = 30% of the exam).

## Key concepts

### Users
An individual identity, usually a real person or an application, with long-term credentials (password + optional access keys).

### Groups
A collection of users sharing the same permissions — attach a policy once to a group instead of to every individual user.

### Roles
A set of permissions that can be **temporarily assumed** — by a user, an AWS service (EC2, Lambda), or an external federated identity (SAML/OIDC). Issues short-lived, auto-expiring credentials via STS — safer than permanent credentials.

### Policies
A policy is a JSON document made up of a list of statements — each statement defines whether a specific action on a specific AWS resource is allowed or denied.
- **Effect**: Allow or Deny
- **Action**: the API operation (e.g., `s3:GetObject`)
- **Resource**: which AWS resource it applies to

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```

## Hands-on / commands

```bash
# Create a user
aws iam create-user --user-name test-user

# Create a group and add user to it
aws iam create-group --group-name developers
aws iam add-user-to-group --user-name test-user --group-name developers

# Attach a managed policy to a group
aws iam attach-group-policy --group-name developers \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Create a role (trust policy required)
aws iam create-role --role-name my-ec2-role \
  --assume-role-policy-document file://trust-policy.json
```

## Common exam gotchas
- **IAM is global**, not region-specific
- **Explicit Deny always wins** — overrides any Allow, no matter how many policies grant access
- Principle of **least privilege** — grant only what's needed, nothing more
- Root account should have MFA enabled and never be used day-to-day

## My notes / things that confused me
_(fill in as you go)_
