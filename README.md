# IT to Cloud Engineering Journey

[LinkedIn](https://www.linkedin.com/in/miguel-martinez-141375336/) · [GitHub Profile](https://github.com/MiguelinIT00)

## About me

I'm Miguel Martinez, an IT support and infrastructure professional developing toward cloud engineering. My background includes supporting Microsoft 365, Active Directory, Google Workspace, Windows and macOS endpoints, Addigy mobile-device management, onboarding and offboarding, SaaS access, asset workflows, and day-to-day user troubleshooting.

That experience shaped how I approach cloud systems: identity must be intentional, network paths should be explainable, changes need verification, and good documentation is part of the solution. I am now applying those habits to AWS while building deeper skills in compute, networking, security, automation, observability, and Infrastructure as Code.

This repository is the working record of that development. It is not a collection of copied definitions or claims about production systems I have not operated. It shows what I have completed, what I have designed as a study exercise, what I can explain, and what I plan to validate next.

The material is deliberately evidence-based:

- **Completed** means I performed the activity and documented the result.
- **Study lab** means the runbook is ready to execute and includes verification and cleanup.
- **Architecture exercise** means I designed and reasoned through a solution; it does not claim a production deployment.
- LocalStack work is identified separately from work performed in an AWS account.

## What I am learning

| Area | AWS services and concepts | Evidence in this repository |
|---|---|---|
| Compute | EC2, AMIs, instance types, EBS, user data, instance lifecycle | [EC2 deep dive](Notes/ec2-basics.md), [EC2 launch lab](labs/ec2-first-launch.md) |
| Networking | VPC, CIDR, subnets, route tables, internet/NAT gateways, security groups, NACLs | [VPC deep dive](Notes/vpc-basics.md), [packet-flow lab](labs/vpc-packet-flow-lab.md) |
| Identity | IAM identities, roles, policies, temporary credentials, least privilege | [IAM deep dive](Notes/iam-basics.md), [EC2-to-S3 lab](labs/iam-ec2-s3-lab.md) |
| Storage | S3, EBS, EFS, encryption, versioning, lifecycle management | [Storage guide](Notes/s3-basics.md) |
| Availability | Elastic Load Balancing, target groups, health checks, Auto Scaling | [Scaling and load balancing](Notes/auto-scaling-and-load-balancing.md) |
| Operations | CloudWatch, CloudTrail, Systems Manager, troubleshooting, tagging | [Operations guide](Notes/observability-and-troubleshooting.md), [troubleshooting scenarios](labs/troubleshooting-scenarios.md) |
| Architecture | Multi-AZ design, fault isolation, security layers, cost tradeoffs | [Highly available web architecture](architecture/highly-available-web-application.md) |
| Infrastructure as Code | CloudFormation resources, parameters, dependencies, change-set review | [EC2 study template](templates/README.md) |
| Local development | LocalStack, Docker, AWS CLI, repeatable experiments | [LocalStack project](projects/aws-cloud-infrastructure-localstack.md) |

## Reference architecture

The main architecture exercise is a highly available web application:

```text
Internet
   |
Route 53 / TLS certificate
   |
Application Load Balancer in two public subnets
   |
Auto Scaling group of EC2 instances in two private subnets
   |
RDS Multi-AZ in isolated database subnets

Supporting services: IAM roles, S3, CloudWatch, CloudTrail,
Systems Manager, Secrets Manager, AWS Backup, and VPC endpoints
```

The design separates public entry points from private compute and data layers. It also explains what happens when an instance or Availability Zone fails, how operators investigate incidents, and where cost and complexity tradeoffs appear.

## Study workflow

1. Learn the service boundary: what problem the service solves and what it does not solve.
2. Draw the request and data flow before building.
3. Build with the smallest safe permissions and network exposure.
4. Verify with commands, logs, metrics, and health checks.
5. Intentionally test a failure condition.
6. Troubleshoot from the outside in: DNS, routing, firewall rules, listener, target, process, dependency.
7. Clean up chargeable resources.
8. Document the result and the decision behind it.

## How this project has developed

### 1. Building the foundation

I started by documenting the purpose of core services such as EC2, S3, IAM, VPC, load balancing, Auto Scaling, and Lambda. The first goal was to understand the responsibility of each service instead of memorizing product names.

### 2. Connecting the services

The notes then moved from isolated definitions to system relationships: an EC2 instance runs inside a VPC, receives permissions through an IAM role, stores durable objects in S3, publishes operational signals, and can sit behind a load balancer as part of an Auto Scaling group.

### 3. Practicing through the console and CLI

I completed an initial EC2 launch and built a local AWS-style environment with Docker, LocalStack, and the AWS CLI. The S3 lab practices a full resource lifecycle—create, verify, upload, retrieve, delete, and confirm cleanup—without presenting local emulation as production AWS experience.

### 4. Thinking like an operator

The project now includes packet-flow analysis, least-privilege tests, failure scenarios, health checks, monitoring, audit visibility, backup thinking, RTO/RPO, and structured troubleshooting. The focus is not only “can I create it?” but also “can I secure it, observe it, diagnose it, recover it, and remove it safely?”

### 5. Moving toward repeatable engineering

The current stage adds architecture exercises and a CloudFormation study template so infrastructure decisions can be reviewed, versioned, validated, and reproduced. Planned work will turn the open study labs into completed evidence with sanitized commands, expected versus actual results, failure tests, and cleanup records.

## Current status

Completed work includes AWS fundamentals, an initial EC2 launch, AWS console exploration, and LocalStack/AWS CLI S3 practice. The deeper labs and architecture documents are study artifacts for the next iteration. Their checklists remain open until each verification step has been performed.

The [project roadmap](projects/roadmap.md) shows which exercises are completed and which still require evidence.

**Build. Break. Troubleshoot. Understand. Improve.**
