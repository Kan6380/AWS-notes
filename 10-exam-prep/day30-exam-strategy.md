# Day 30 — SAA-C03 Exam Domains, Scoring, and Study Strategy

## What it is
A summary of how the AWS SAA-C03 exam is structured and scored, plus a practical strategy for final prep.

## Exam structure
- **65 total questions** — 50 scored + 15 unscored (you can't tell which are which, answer everything with full effort)
- **130 minutes**
- **Pass mark: 720 / 1000** (scaled score)
- **Compensatory scoring** — total score across all domains must reach 720; a strong domain can offset a weaker one, but Security (30%) is hard to recover from if badly failed

## Domain weights

| Domain | Weight |
|---|---|
| Design Secure Architectures | 30% |
| Design Resilient Architectures | 26% |
| Design High-Performing Architectures | 24% |
| Design Cost-Optimized Architectures | 20% |

## Study strategy

1. **Weight study time to match domain weights** — Security and Resilience alone are over half the exam
2. **Take one practice exam early** to identify actual gaps, instead of re-studying everything blindly
3. **Prioritize practice questions over video courses** if you already have hands-on experience — the exam tests scenario judgment and AWS-specific phrasing, not raw knowledge
4. **Review every wrong answer AND every lucky-guess right answer** — understand the "why," not just the "what"
5. **Final 3–4 days**: no new content, just timed full-length practice exams + review

## Common gotcha topics to double check before the exam
- Multi-AZ vs Read Replicas (RDS)
- Security Groups vs NACLs (stateful vs stateless)
- S3 storage classes and lifecycle transitions
- IAM Roles vs Users, federation (SAML/OIDC)
- VPC Peering non-transitivity
- NAT Gateway vs Internet Gateway
- Disaster recovery patterns: backup & restore, pilot light, warm standby, active-active
- Spot vs Reserved vs Savings Plans vs On-Demand

## My notes / things that confused me
_(fill in as you go — use this file as your final pre-exam review checklist)_
