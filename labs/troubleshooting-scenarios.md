# Cloud Troubleshooting Scenarios

Use each scenario as a spoken drill: symptom, evidence, hypothesis, smallest safe change, verification, and prevention.

## 1. ALB returns 503 and has no healthy targets

Check target registration, health-check port/path/success codes, ALB-to-target security-group rules, process listener, host firewall, and application logs. A running EC2 instance is not proof of a healthy target.

**Prevention:** deployment health gates, a reliable readiness endpoint, alarms on healthy-host count, and tested rollback.

## 2. EC2 in a private subnet cannot download updates

Check the private subnet route-table association, default route to a working NAT gateway, NAT placement in a public subnet, public subnet route to the internet gateway, Elastic IP, SG/NACL egress and return traffic, DNS resolution, and whether a VPC endpoint is the intended path.

**Tradeoff:** a NAT gateway per AZ improves zonal independence but costs more. Supported VPC endpoints can keep service traffic private and may reduce NAT processing.

## 3. Application receives S3 `AccessDenied`

Identify the caller with STS. Check action/resource ARN, identity policy, bucket policy, endpoint policy, permissions boundary, SCP, conditions, and KMS permissions. Do not solve the incident by attaching administrator access.

**Prevention:** policy tests for required allow and deny cases, role-based temporary credentials, and reviewed least-privilege templates.

## 4. CPU is low but the website is slow

CPU is only one signal. Check ALB target latency/errors, application traces/logs, memory, disk, connection pools, thread/worker saturation, database locks and slow queries, DNS, and downstream APIs.

**Prevention:** service-level indicators, dependency telemetry, load testing, and alerts tied to user outcomes.

## 5. Auto Scaling launches instances that immediately fail

Check the launch template version, AMI availability, capacity/quotas, subnet IP capacity, security groups, IAM instance profile, user-data logs, package repositories, secrets/configuration, and target health-check grace/warm-up settings.

**Prevention:** versioned AMIs and launch templates, canary validation, boot-time telemetry, and rollback to a known-good version.

## 6. A stopped/started EC2 server has a different public address

An automatically assigned public IPv4 address can change across stop/start. Use stable DNS and architecture-level endpoints; use an Elastic IP only when a stable instance address is actually required.

**Prevention:** avoid coupling clients directly to replaceable instance addresses.

## 7. Database connections time out after a network change

Check name resolution, application-to-database route path, database SG source reference, NACL return traffic, database status, listener/port, connection limits, secrets rotation, and whether the client uses the correct endpoint.

**Prevention:** connectivity tests, controlled changes, flow logs, database alarms, and a tested rollback.

## Interview drill template

For each scenario, speak this structure:

> I would first confirm the scope and recent changes. Then I would follow the request path and gather evidence at each boundary. My first hypothesis would be __ because __. I would test it with __. The smallest reversible mitigation is __. I would verify recovery using the user-facing signal __, then create the prevention action __.
