# AWS and Cloud Engineering Fundamentals

## Cloud is an operating model

Cloud engineering is not only moving virtual machines to someone else's data center. It changes how infrastructure is requested, automated, measured, secured, scaled, and paid for.

Important characteristics include:

- resources available through APIs and self-service workflows;
- elastic capacity that can change with demand;
- usage-based pricing and the need for cost ownership;
- global Regions and isolated Availability Zones;
- managed services that shift some operational work to the provider;
- automation and Infrastructure as Code instead of one-off manual configuration.

## Service models

| Model | Customer manages | Provider manages | AWS-oriented example |
|---|---|---|---|
| IaaS | Guest OS, runtime, application, data, and configuration | Physical facilities, hardware, and virtualization | EC2 |
| PaaS/managed platform | Application, data, and service configuration | More of the OS, runtime, scaling, or maintenance | Elastic Beanstalk, RDS |
| SaaS | User access, data use, and configuration | The full application stack | A finished business application |

The shared responsibility boundary changes by service. Managed does not mean responsibility-free.

## Global infrastructure

- A **Region** is a separate geographic area.
- An **Availability Zone** is an isolated location inside a Region with independent infrastructure.
- An **edge location** supports services such as content delivery and DNS closer to users.

Choose a Region based on business requirements: latency, regulatory constraints, service availability, resilience strategy, and cost.

## Six Well-Architected pillars

1. Operational excellence
2. Security
3. Reliability
4. Performance efficiency
5. Cost optimization
6. Sustainability

An engineering decision is usually a tradeoff. Multi-AZ resources improve resilience but can add cost. A managed database reduces undifferentiated operational work but may offer less low-level control than self-managing one on EC2.

## Core service map

| Need | Common AWS starting point |
|---|---|
| Virtual server | EC2 |
| Object storage | S3 |
| Persistent block volume | EBS |
| Shared file system | EFS |
| Private network | VPC |
| Identity and authorization | IAM |
| Relational database | RDS/Aurora |
| DNS | Route 53 |
| HTTP load balancing | Application Load Balancer |
| Metrics, logs, alarms | CloudWatch |
| API audit history | CloudTrail |
| Templates for infrastructure | CloudFormation |
| Event-driven functions | Lambda |

## Design questions before selecting a service

- What user or business outcome must the system provide?
- What availability, RTO, and RPO are required?
- What traffic pattern and data volume are expected?
- Is state local, shared, or durable?
- What data is sensitive and who should access it?
- What must be logged, audited, backed up, and retained?
- How will the team deploy, patch, monitor, and recover it?
- Which component is likely to become the bottleneck?
- What is the cost when idle and at peak?

## My mental model

```text
Identity decides who may act.
Networking decides where traffic may flow.
Compute runs the workload.
Storage and databases hold state.
Observability explains what the system is doing.
Automation makes the environment repeatable.
```

## Official references

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/)
- [AWS global infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)
