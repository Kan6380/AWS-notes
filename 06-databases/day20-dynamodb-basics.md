# Day 20 — DynamoDB Basics (NoSQL vs Relational)

## What it is
**DynamoDB** is AWS's fully managed **NoSQL** key-value/document database — built for massive scale and single-digit millisecond latency, with no servers to manage at all (fully serverless).

## Why it matters
Many modern, high-scale applications (especially serverless ones, often paired with Lambda) use DynamoDB instead of RDS. Understanding when to pick one over the other is a common interview and exam topic.

## Key concepts

### NoSQL vs Relational (RDS)

| | RDS (Relational) | DynamoDB (NoSQL) |
|---|---|---|
| **Schema** | Fixed, defined upfront (tables, columns, types) | Flexible — each item can have different attributes |
| **Relationships** | Joins across tables | No native joins — data often denormalized |
| **Scaling** | Vertical (bigger instance) mainly, some read scaling via replicas | Horizontal — scales seamlessly to massive throughput |
| **Best for** | Complex queries, transactions, structured relational data | High-scale, simple key-based lookups, unpredictable/huge traffic |
| **Server management** | You choose instance size, still "a server" | Fully serverless — no instances at all |

### Core DynamoDB concepts
- **Table** — collection of items (like a table of rows, but schema-flexible)
- **Item** — a single record (like a row)
- **Primary Key** — either just a Partition Key, or a Partition Key + Sort Key (composite)
- **Capacity modes**:
  - **On-Demand** — pay per request, no capacity planning
  - **Provisioned** — set read/write capacity units upfront, cheaper at predictable scale

## Hands-on / commands

```bash
# Create a table
aws dynamodb create-table \
  --table-name Tasks \
  --attribute-definitions AttributeName=TaskId,AttributeType=S \
  --key-schema AttributeName=TaskId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST

# Insert an item
aws dynamodb put-item \
  --table-name Tasks \
  --item '{"TaskId": {"S": "1"}, "Title": {"S": "Learn DynamoDB"}}'

# Get an item
aws dynamodb get-item \
  --table-name Tasks \
  --key '{"TaskId": {"S": "1"}}'
```

## Common exam gotchas
- DynamoDB is the go-to answer for "serverless," "millisecond latency at massive scale," or "unpredictable/huge traffic spikes" scenarios
- No native SQL joins — application logic (or denormalized design) handles relationships instead
- Global Tables feature enables multi-region replication for DynamoDB — worth knowing exists, even at a high level

## My notes / things that confused me
_(fill in as you go)_
