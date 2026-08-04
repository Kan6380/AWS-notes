# Day 26 — CloudWatch — Monitoring, Logs, Alarms

## What it is
**CloudWatch** is AWS's native monitoring and observability service — collects metrics, logs, and lets you set alarms that trigger actions (notifications, auto scaling, etc.).

## Why it matters
Almost every AWS service integrates with CloudWatch automatically. It's the default answer for "how do I know if something's wrong" across the entire platform — conceptually the AWS-native equivalent of a Loki + Grafana observability stack 

## Key concepts

### CloudWatch Metrics
Numeric data points over time (CPU utilization, request count, error rate). Most AWS services push metrics automatically; you can also publish custom metrics.

### CloudWatch Logs
Centralized log storage. Services like Lambda, ECS, and EC2 (via the CloudWatch Agent) can send logs here. Organized into **Log Groups** and **Log Streams**.

### CloudWatch Alarms
Watch a metric, and trigger an action when a threshold is breached — e.g., "if CPU > 80% for 5 minutes, send an SNS notification" or "trigger an Auto Scaling action."

### CloudWatch Dashboards
Custom visual dashboards combining multiple metrics/graphs — similar purpose to a Grafana dashboard.

## Hands-on / commands

```bash
# List available metrics for EC2
aws cloudwatch list-metrics --namespace AWS/EC2

# Create an alarm (e.g., CPU > 80%)
aws cloudwatch put-metric-alarm \
  --alarm-name high-cpu \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --statistic Average \
  --period 300 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --evaluation-periods 2 \
  --dimensions Name=InstanceId,Value=i-xxxxxxxx \
  --alarm-actions arn:aws:sns:eu-west-1:123456789012:my-sns-topic

# Tail logs from a log group
aws logs tail /aws/lambda/my-function --follow
```

## Common exam gotchas
- CloudWatch is **regional** — metrics/logs from one region don't automatically appear in another
- Detailed monitoring (1-minute granularity) costs extra vs. basic monitoring (5-minute granularity, free)
- CloudWatch Alarms can trigger Auto Scaling actions, SNS notifications, or Lambda functions — a common exam scenario chains these together (e.g., "alert on high error rate AND auto-remediate via Lambda")

## My notes / things that confused me
good place to note the parallels/differences with your Grafana Explore workflow that caught the Postgres readiness probe bug"
After: "good place to note parallels with any Grafana/Loki debugging workflow you've used
