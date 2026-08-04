# Day 3 — Installing & Configuring AWS CLI

## What it is
The **AWS CLI (Command Line Interface)** lets you interact with AWS services from your terminal instead of clicking through the Console — faster, scriptable, and essential for automation/CI-CD work.

## Why it matters
Almost every real-world AWS workflow (including CI/CD pipelines like GitHub Actions) uses the CLI or SDKs, not the Console. Learning it early builds habits that carry directly into DevOps work.

## Key concepts

- **AWS CLI v2** — current version, install per OS
- **Access Key ID / Secret Access Key** — credentials the CLI uses to authenticate as an IAM user
- **Named profiles** — let you configure multiple AWS accounts/roles on one machine
- **Default region/output format** — configured once, used for every command unless overridden

## Hands-on / commands

### Install (Linux/Ubuntu — matches homelab environment)
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

### Install (Mac)
```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### Install (Windows)
Download and run the MSI installer from AWS docs.

### Configure
```bash
aws configure
```
You'll be prompted for:
```
AWS Access Key ID: 
AWS Secret Access Key: 
Default region name: eu-west-1
Default output format: json
```

### Test it works
```bash
aws sts get-caller-identity
```
This returns your account ID, user ARN, and confirms your credentials are valid.

### Using named profiles (multiple accounts)
```bash
aws configure --profile myproject
aws s3 ls --profile myproject
```

## Common exam gotchas
- Never hardcode Access Keys into scripts or commit them to Git — this is a huge security anti-pattern (relevant to your earlier `git filter-repo`/BFG secret remediation notes)
- Long-lived Access Keys are discouraged in modern best practice — prefer IAM Roles + temporary credentials (via STS) where possible, especially for CI/CD (ties into the OIDC federation topic later)
- `aws configure` credentials are stored in plaintext at `~/.aws/credentials` — fine for local dev, not something to share or commit

## My notes / things that confused me
_(fill in as you go)_
