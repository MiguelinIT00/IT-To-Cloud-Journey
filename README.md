# IT to Cloud Engineering Journey

This repository documents my transition from enterprise IT systems and infrastructure into cloud engineering. I am building on experience with endpoints, identity, networking, and troubleshooting while learning how to design, secure, operate, and automate AWS workloads.

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

## Current status

Completed work includes AWS fundamentals, an initial EC2 launch, AWS console exploration, and LocalStack/AWS CLI S3 practice. The deeper labs and architecture documents are study artifacts for the next iteration. Their checklists remain open until each verification step has been performed.

The [project roadmap](projects/roadmap.md) shows which exercises are completed and which still require evidence.

**Build. Break. Troubleshoot. Understand. Improve.**
