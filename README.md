☁️ AWS Notes — Learning in Public

Personal notes repo documenting my journey learning AWS from the ground up — covering core concepts, hands-on labs, and prep for the AWS Certified Solutions Architect Associate (SAA-C03) exam.

Each folder/file corresponds to a topic, written in beginner-friendly language with examples, diagrams (where useful), and commands I've actually run myself.

🎯 Goals
Build a strong foundation in AWS core services
Document hands-on labs in a way I (and others) can revisit
Prepare for and pass AWS SAA-C03
Use this repo as a reference while working toward Cloud/DevOps engineering roles
🗂️ How this repo is organized
aws-notes/
├── README.md                  ← you are here
├── 01-fundamentals/
├── 02-compute/
├── 03-storage/
├── 04-networking/
├── 05-iam-security/
├── 06-databases/
├── 07-containers-serverless/
├── 08-monitoring-cost/
├── 09-devops-iac/
└── 10-exam-prep/

Each note file follows a simple template:

markdown
# Topic Name

## What it is (simple explanation)

## Why it matters / when to use it

## Key concepts

## Hands-on steps / commands

## Common exam gotchas

## My notes / things that confused me
📅 Day-by-Day Index

Pace: adjust freely. Each "day" is roughly 30–60 minutes of focused reading + hands-on practice.

Week 1 — Fundamentals & Core Compute
Day	Topic	Folder
1	What is AWS? Cloud computing basics, regions & availability zones	01-fundamentals/what-is-aws.md
2	AWS Free Tier, AWS Console tour, billing basics	01-fundamentals/console-billing.md
3	Installing & configuring AWS CLI	01-fundamentals/aws-cli-setup.md
4	What is EC2? Instance types, AMIs	02-compute/ec2-basics.md
5	Creating your first EC2 instance (step by step)	02-compute/creating-ec2.md
6	Security Groups vs NACLs	02-compute/security-groups-vs-nacls.md
7	Elastic IPs, Key Pairs, connecting via SSH	02-compute/ec2-connectivity.md
Week 2 — Storage & Networking
Day	Topic	Folder
8	What is S3? Buckets vs Objects	03-storage/s3-basics.md
9	Creating a bucket, uploading files, static website hosting	03-storage/s3-hands-on.md
10	S3 storage classes & lifecycle policies	03-storage/s3-storage-classes.md
11	EBS vs EFS vs S3 (storage comparison)	03-storage/storage-comparison.md
12	What is a VPC? Subnets, route tables	04-networking/vpc-basics.md
13	Internet Gateway, NAT Gateway	04-networking/igw-nat.md
14	VPC Peering, VPC Endpoints	04-networking/vpc-peering-endpoints.md
Week 3 — Identity, Security & Databases
Day	Topic	Folder
15	What is IAM? Users, Groups, Roles, Policies	05-iam-security/iam-basics.md
16	IAM Roles vs Users (deep dive)	05-iam-security/roles-vs-users.md
17	MFA, root account security best practices	05-iam-security/mfa-security.md
18	Identity Federation — SAML & OIDC	05-iam-security/federation-saml-oidc.md
19	What is RDS? Multi-AZ vs Read Replicas	06-databases/rds-basics.md
20	DynamoDB basics (NoSQL vs relational)	06-databases/dynamodb-basics.md
21	Database backup & snapshot strategies	06-databases/backups-snapshots.md
Week 4 — Containers, Serverless & Automation
Day	Topic	Folder
22	What is Lambda? Serverless basics	07-containers-serverless/lambda-basics.md
23	ECS & Fargate fundamentals	07-containers-serverless/ecs-fargate.md
24	EKS fundamentals (Kubernetes on AWS)	07-containers-serverless/eks-basics.md
25	ALB, Auto Scaling Groups, Launch Templates	02-compute/alb-asg.md
26	CloudWatch — monitoring, logs, alarms	08-monitoring-cost/cloudwatch-basics.md
27	Cost optimization — Savings Plans, Reserved vs Spot	08-monitoring-cost/cost-optimization.md
28	CloudFormation basics — Infrastructure as Code	09-devops-iac/cloudformation-basics.md
29	CI/CD on AWS — CodePipeline overview + GitHub Actions + OIDC	09-devops-iac/cicd-oidc.md
30	SAA-C03 exam domains, scoring, and study strategy	10-exam-prep/exam-strategy.md
📖 Topics covered (quick reference)
Fundamentals — cloud computing, regions/AZs, AWS CLI
Compute — EC2, Auto Scaling, Load Balancers
Storage — S3, EBS, EFS
Networking — VPC, Subnets, Gateways, Peering
Security & Identity — IAM, MFA, Federation (SAML/OIDC)
Databases — RDS, DynamoDB
Containers & Serverless — Lambda, ECS, EKS, Fargate
Monitoring & Cost — CloudWatch, Cost Explorer, pricing models
DevOps & IaC — CloudFormation, CI/CD, OIDC federation for pipelines
Exam Prep — SAA-C03 domain breakdown, practice question notes
