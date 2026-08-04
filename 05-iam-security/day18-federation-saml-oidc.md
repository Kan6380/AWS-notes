# Day 18 — Identity Federation — SAML & OIDC

## What it is
Federation lets you manage identities in an **external system** (like Azure AD, Okta, or GitHub) and grant those identities temporary access to AWS resources — without creating a duplicate IAM User for each one.

## Why it matters
At scale, managing separate AWS credentials for every employee/app is a nightmare (password sprawl, offboarding gaps, duplicated identity management). Federation solves this by trusting an existing identity source.

## Key concepts

### SAML (Security Assertion Markup Language)
- XML-based
- Used for enterprise SSO into the AWS Console
- Classic use case: Azure AD (or Okta/ADFS) authenticates a user, sends AWS a signed SAML assertion, AWS trusts it and issues temporary credentials
- API call: `AssumeRoleWithSAML`

### OpenID Connect (OIDC)
- JSON-based (JWT tokens), built on OAuth 2.0
- Used for modern web/mobile apps and **CI/CD pipelines**
- Classic use case: GitHub Actions authenticating to AWS without storing long-lived access keys as secrets
- API call: `AssumeRoleWithWebIdentity`

### Comparison

| | SAML | OIDC |
|---|---|---|
| Format | XML | JSON (JWT) |
| Typical use | Enterprise SSO → AWS Console | CI/CD pipelines, modern apps |
| Common IdPs | Azure AD, Okta, ADFS | GitHub Actions, Google, Cognito |
| AWS mechanism | AssumeRoleWithSAML | AssumeRoleWithWebIdentity |

## Hands-on / commands

```bash
# Create a SAML identity provider in IAM
aws iam create-saml-provider \
  --saml-metadata-document file://azure-ad-metadata.xml \
  --name AzureADProvider

# Create an OIDC identity provider (e.g., for GitHub Actions)
aws iam create-open-id-connect-provider \
  --url https://token.actions.githubusercontent.com \
  --client-id-list sts.amazonaws.com \
  --thumbprint-list <thumbprint>
```

Example trust policy for a GitHub Actions OIDC role (restricts to a specific repo):
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789012:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:sub": "repo:Kan6380/task-tracker-3tier-devops:ref:refs/heads/main"
        }
      }
    }
  ]
}
```

## Common exam gotchas
- Federated identities map to an IAM **Role**, never a User
- Both mechanisms issue **temporary** credentials — no static long-lived keys involved
- GitHub Actions + OIDC is a modern best practice replacing storing AWS access keys as GitHub Secrets — worth remembering for real-world CI/CD security, even though it's less exam-core

## My notes / things that confused me
_(fill in as you go — this connects directly to your Azure AD Connect / duplicate-user incident story: same underlying trust relationship concept, different direction)_
