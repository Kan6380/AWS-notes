# Day 10 — S3 Storage Classes & Lifecycle Policies

## What it is
S3 offers multiple **storage classes** with different cost/retrieval-speed tradeoffs, and **lifecycle policies** to automatically move objects between them (or delete them) over time.

## Why it matters
This is a heavily tested SAA-C03 topic (falls under the Cost-Optimized Architectures domain) — knowing which class fits which access pattern saves real money.

## Key concepts

| Storage Class | Use case | Retrieval |
|---|---|---|
| **S3 Standard** | Frequently accessed data | Instant |
| **S3 Intelligent-Tiering** | Unknown/changing access patterns | Instant (auto-moves between tiers) |
| **S3 Standard-IA** (Infrequent Access) | Accessed less often but needs fast retrieval when needed | Instant, but retrieval fee |
| **S3 One Zone-IA** | Infrequent access, non-critical (only 1 AZ, cheaper) | Instant, retrieval fee |
| **S3 Glacier Instant Retrieval** | Archive, but occasionally needs fast access | Instant |
| **S3 Glacier Flexible Retrieval** | Archive, rarely accessed | Minutes to hours |
| **S3 Glacier Deep Archive** | Long-term archive, cheapest option | Up to 12 hours |

### Lifecycle Policies
Rules that automatically transition or expire objects based on age. Example: "move to Standard-IA after 30 days, move to Glacier after 90 days, delete after 365 days."

## Hands-on / commands

```bash
# Upload directly to a specific storage class
aws s3 cp myfile.txt s3://my-bucket/ --storage-class STANDARD_IA

# View/set lifecycle policy (via JSON config file)
aws s3api put-bucket-lifecycle-configuration \
  --bucket my-bucket \
  --lifecycle-configuration file://lifecycle.json
```

Example `lifecycle.json`:
```json
{
  "Rules": [
    {
      "ID": "MoveToIAThenGlacier",
      "Status": "Enabled",
      "Filter": {"Prefix": ""},
      "Transitions": [
        { "Days": 30, "StorageClass": "STANDARD_IA" },
        { "Days": 90, "StorageClass": "GLACIER" }
      ],
      "Expiration": { "Days": 365 }
    }
  ]
}
```

## Common exam gotchas
- Standard-IA and One Zone-IA charge a **retrieval fee** — not free just because storage cost is lower
- Objects must be stored a minimum duration (e.g., 30 days for IA) before you can transition them — early transition/deletion can incur early-deletion fees
- Intelligent-Tiering has no retrieval fees and no minimum duration — good default when access patterns are unpredictable

## My notes / things that confused me
_(fill in as you go)_
