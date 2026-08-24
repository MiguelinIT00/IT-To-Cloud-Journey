# Architecture Exercise: Highly Available AWS Web Application

**Status:** Design exercise for study and interview discussion. This document does not claim a production deployment.

## Requirements assumed for the exercise

- Public HTTPS web application.
- Survive loss of one application instance and tolerate one AZ failure at the web/app tiers.
- Relational durable data.
- No direct inbound administrative access to application instances.
- Central metrics, logs, API audit history, backups, and defined cleanup/cost ownership.

Requirements would be validated with stakeholders before implementation, especially availability target, peak traffic, data classification, RTO, RPO, and budget.

## Logical design

```text
Users
  |
Route 53
  |
AWS WAF (when justified) + Application Load Balancer
  |                           public subnets in AZ A and AZ B
  +-------------------+
  |                   |
EC2 app A           EC2 app B  Auto Scaling group, private subnets
  |                   |
  +------ RDS/Aurora Multi-AZ ----- isolated database subnets

EC2 role -> S3 / Secrets Manager / CloudWatch
Operators -> IAM Identity Center -> Systems Manager Session Manager
Audit -> CloudTrail; configuration -> AWS Config
```

## Request flow

1. Route 53 resolves the application name.
2. The client establishes TLS with the public Application Load Balancer.
3. The ALB listener applies configured routing and sends the request to a healthy target.
4. The application security group accepts the app port only from the ALB security group.
5. The application obtains temporary AWS credentials from its instance role.
6. The app connects to the database using a secret retrieved at runtime and a database endpoint.
7. The database security group accepts its port only from the app security group.
8. Logs and metrics go to central services; API activity is recorded by CloudTrail.

## Subnet and routing design

| Tier | Subnets | Default route |
|---|---|---|
| Load balancer/NAT | Public subnet in each AZ | Internet gateway |
| Application | Private subnet in each AZ | Same-AZ NAT gateway when general IPv4 egress is required |
| Database | Isolated subnet in each AZ | No general internet route |

Use S3 gateway and other appropriate VPC endpoints when private service access, policy control, or NAT-cost avoidance justifies them.

## Security decisions

- TLS at the load balancer with managed certificate lifecycle.
- No direct internet route or public address on application/database resources.
- SG-to-SG references express allowed tier relationships.
- Session Manager instead of public SSH/RDP where possible.
- EC2 instance role with least privilege and temporary credentials.
- Secrets Manager/Parameter Store for secrets; no secrets in user data, AMIs, or Git.
- Encryption for EBS, S3, database storage, backups, and relevant logs.
- CloudTrail, Config, logs, and alerts protected with retention and access controls.
- WAF/rate controls based on threat model, not as a substitute for secure application code.

## Reliability decisions

- ALB and ASG use at least two AZs.
- Desired capacity permits loss of one instance without total outage.
- App instances are replaceable and do not hold exclusive durable state.
- Health checks represent readiness and deployment includes rollback.
- Database uses a managed Multi-AZ option and tested backups.
- RTO/RPO determine backup schedule, retention, and recovery design.
- Quotas and subnet IP capacity are monitored.

## Operations

Minimum dashboard/alarms:

- availability and successful-request rate;
- ALB latency, 4xx/5xx categories, healthy targets, and rejected connections;
- ASG capacity, failed launches, and scaling activity;
- EC2 CPU plus collected memory/disk/application metrics;
- database CPU, storage, connections, latency, replica/failover signals as applicable;
- synthetic user check from outside the VPC;
- estimated spend and budget alerts.

Deployment should be automated, observable, versioned, and reversible. Infrastructure definitions and application artifacts should move through review and test stages.

## Failure walkthroughs

### One EC2 instance fails

The health check removes it from service, the ALB sends traffic to healthy targets, and the ASG launches a replacement. Capacity and latency alarms verify whether remaining targets can handle traffic.

### One AZ becomes unavailable

The load balancer uses healthy targets in the other AZ. The design needs sufficient remaining capacity, independent zonal routing/NAT behavior, and a database failover plan. “Multi-AZ” must be validated per service, not assumed globally.

### Bad application deployment

New targets fail readiness checks or user-error signals rise. Deployment pauses/rolls back to a known-good artifact. Logs and change metadata correlate the failure with the release.

### Database failure

The managed database performs its configured failover. Clients must reconnect with sane timeouts/backoff. Recovery is verified at the application layer, not only by database status.

## Cost discussion

Likely fixed/step costs include the load balancer, NAT gateways, database, log ingestion/retention, and minimum EC2 capacity. Cost controls include right-sizing from measurements, nonproduction schedules, appropriate purchasing models, log retention, S3 lifecycle rules, VPC endpoints when economically justified, and automatic cleanup of short-lived environments.

A single-AZ, single-instance learning environment is cheaper but does not meet the availability assumptions. State that tradeoff explicitly.

## Alternatives

- ECS/Fargate or EKS when container orchestration requirements justify them.
- Lambda/API Gateway for event-driven or bursty functions that fit their execution model.
- CloudFront for global caching and edge delivery.
- DynamoDB when access patterns and data model favor a managed key-value/document database.
- Aurora/serverless database options when workload and cost patterns fit.

Service selection follows requirements; it is not a contest to use the most AWS products.

## Validation plan

- [ ] Load test normal and peak behavior.
- [ ] Terminate one target and measure recovery.
- [ ] Simulate or game-day a zonal failure safely.
- [ ] Perform database failover and restore-from-backup tests.
- [ ] Validate least-privilege allow and deny cases.
- [ ] Scan templates and images for vulnerabilities/misconfiguration.
- [ ] Verify alarms, runbooks, escalation, and rollback.
- [ ] Review the architecture against all six Well-Architected pillars.
