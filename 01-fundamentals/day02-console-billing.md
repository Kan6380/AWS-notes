# Day 2 — AWS Free Tier, Console Tour, Billing Basics

## What it is
The **AWS Free Tier** lets new accounts use certain services for free, up to specific limits, for 12 months (some services are "always free" beyond that). The **AWS Console** is the web-based dashboard for managing all your AWS resources.

## Why it matters
Understanding free tier limits and billing basics from day one prevents surprise charges — a very common beginner fear. Knowing where to check costs also builds good habits before you start deploying real resources.

## Key concepts

- **Free Tier types:**
  - 12 months free (e.g., 750 hrs/month of t2.micro EC2)
  - Always free (e.g., limited Lambda invocations)
  - Trials (short-term free access to some services)
- **Billing Dashboard** — shows current month's spend, forecasted spend, service-by-service breakdown
- **Budgets** — set spending alerts so you get notified before costs run away
- **Cost Explorer** — visualize spending trends over time

## Hands-on / commands

1. Log into AWS Console → search "Billing" in the top search bar
2. Go to **Billing Dashboard** → review current charges
3. Go to **Budgets** → create a budget (e.g., alert me if spend exceeds $5)
4. Go to **Free Tier** page → review what's included and how much you've used
5. Bookmark **Cost Explorer** for later use

## Common exam gotchas
- Free Tier limits are **per account**, not per resource — easy to accidentally exceed if you spin up multiple instances
- Some services (like NAT Gateway, data transfer) are **not** covered by Free Tier and can rack up charges quickly — a classic beginner "surprise bill" trap
- Setting a Budget alert does **not** stop spending — it only notifies you. To hard-stop spend you'd need more advanced controls (Service Control Policies, budget actions)

## My notes / things that confused me
_(fill in as you go)_
