# Cloud Engineering Interview Guide

This guide is for speaking clearly about AWS without pretending a study lab was a production system. The strongest interview answers connect a technical choice to reliability, security, operations, and cost.

## 60-second introduction

> My background is in IT systems and infrastructure, including endpoints, identity, networking, and troubleshooting. I am now applying those foundations to cloud engineering. In AWS I have been studying and documenting how EC2, VPC, IAM, S3, load balancing, Auto Scaling, and monitoring work together. I have used the AWS console for foundational labs and LocalStack with Docker and the AWS CLI for repeatable local practice. My current architecture exercise is a multi-AZ web application with an Application Load Balancer, private EC2 instances, and a managed database. What I bring is a support engineer's troubleshooting discipline, and what I am building is stronger automation, infrastructure-as-code, and cloud architecture depth.

Adjust this only to match work you can explain and demonstrate.

## A framework for almost every answer

When asked to design or troubleshoot something, answer in this order:

1. **Requirements:** traffic, availability target, data sensitivity, recovery needs, and budget.
2. **Architecture:** name the components and explain the request/data flow.
3. **Security:** identity, least privilege, network boundaries, encryption, and secrets.
4. **Reliability:** multiple Availability Zones, health checks, backups, and recovery.
5. **Operations:** logs, metrics, alarms, deployment, patching, and incident response.
6. **Cost:** right-sizing, scaling, managed-service tradeoffs, and resource cleanup.

This keeps an answer from becoming a list of AWS service names.

## Explain EC2 like an engineer

Amazon EC2 provides resizable virtual compute instances. A launch decision includes:

- an **AMI** for the operating system and starting software;
- an **instance family and size** for CPU, memory, network, and accelerator needs;
- a **subnet** and network interface inside a VPC;
- one or more **security groups** for stateful traffic filtering;
- **EBS volumes** for persistent block storage, when used;
- an **IAM role** so software can obtain temporary AWS credentials;
- **user data** or a configuration system to bootstrap the host;
- monitoring, patching, backups, and tags for operations.

Stopping an EBS-backed instance normally preserves its EBS volumes and private IPv4 address, while a public IPv4 address can change after stop/start unless an Elastic IP is used. Terminating is final for the instance; volume retention depends on each volume's `DeleteOnTermination` setting.

### Instance selection

- General purpose: balanced workloads and many web/application servers.
- Compute optimized: CPU-intensive batch processing, gaming, or high-performance web tiers.
- Memory optimized: in-memory databases, caches, and memory-heavy analytics.
- Storage optimized: workloads needing high local storage throughput or IOPS.
- Accelerated computing: GPU or specialized accelerator workloads.

A good answer says: measure the workload, choose a reasonable starting family, observe real utilization, and right-size. Do not select solely by CPU count.

## Walk a packet through a VPC

For a public web application:

1. DNS resolves the application name to the load balancer.
2. The client connects to an Application Load Balancer in public subnets.
3. The public subnet route table has a default route to an internet gateway.
4. The load balancer security group permits HTTPS from the intended clients.
5. The listener forwards to a target group.
6. The application security group permits the application port only from the load balancer security group.
7. EC2 instances run in private subnets and do not require inbound internet access.
8. The application connects to the database on its database port; the database security group permits only the application security group.
9. Responses follow established stateful security-group connections back to the client.

A subnet is public because its route table has a route to an internet gateway, not simply because someone named it `public`. For IPv4 internet access, a resource also needs a public IPv4 address or an intermediary such as a public load balancer.

## Security groups versus network ACLs

| Security group | Network ACL |
|---|---|
| Attached to network interfaces/resources | Applied at the subnet boundary |
| Stateful: response traffic is tracked | Stateless: inbound and outbound rules must both allow traffic |
| Allow rules only | Supports allow and deny rules |
| Evaluates all rules | Evaluates numbered rules in order |
| Primary day-to-day resource firewall | Coarse subnet guardrail or explicit blocking use cases |

## IAM answer that goes beyond definitions

Authentication establishes who or what the principal is. Authorization evaluates whether the requested action is allowed. IAM policies describe allowed or denied actions, resources, and optional conditions.

For workloads, prefer roles and temporary credentials over long-lived access keys. Give the EC2 instance an instance profile, scope its policy to the exact S3 bucket and actions required, and use conditions when useful. An explicit deny overrides an allow. Effective access can also be limited by resource policies, permissions boundaries, session policies, service control policies, and endpoint policies.

For people, use federation/central workforce access and MFA where possible. Protect the root user and avoid using it for daily administration.

## High availability and scaling

An Auto Scaling group keeps a desired number of EC2 instances running and can replace unhealthy instances. It can also change capacity through target tracking, step scaling, scheduled scaling, or predictive scaling. An Application Load Balancer distributes HTTP/HTTPS requests across healthy targets and can route by host or path.

For Availability Zone resilience:

- use subnets in at least two AZs;
- place the load balancer and Auto Scaling group across those AZs;
- keep application instances stateless;
- store durable shared data outside instance-local storage;
- use a Multi-AZ database or another data design with a tested failover path;
- test that health checks represent real application health.

Auto Scaling is not the same thing as high availability. Scaling adds or removes capacity; availability also requires fault isolation, redundancy, health detection, and recovery.

## Observability and audit

- **CloudWatch metrics:** numeric time-series such as CPU utilization, request count, latency, and errors.
- **CloudWatch Logs:** application, operating system, and service log data.
- **CloudWatch alarms:** actions or notifications when a metric crosses a threshold.
- **CloudTrail:** records supported AWS API activity for governance and investigation.
- **AWS Config:** evaluates resource configuration and configuration history.
- **VPC Flow Logs:** metadata about accepted and rejected network flows.
- **Systems Manager:** remote management, patching, inventory, and Session Manager access without opening inbound SSH.

Start monitoring from the user outcome: availability, latency, traffic, errors, and saturation. CPU alone does not prove an application is healthy.

## Troubleshooting: website on EC2 is unreachable

Work from the outside in and change one thing at a time:

1. Confirm the exact symptom, start time, scope, and recent changes.
2. Check DNS resolution and whether the client reaches the expected endpoint.
3. Check load balancer listener, certificate, target group health, and health-check path.
4. Check routes: subnet association, internet/NAT gateway where required, and return path.
5. Check security groups and NACLs in both directions as appropriate.
6. Confirm the instance is running and passed EC2 status checks.
7. Confirm the application process is listening on the expected address and port.
8. Inspect application/system logs, memory, disk, and dependency connectivity.
9. Use metrics and logs to validate the hypothesis before and after the change.
10. Record root cause, remediation, and a prevention action such as an alarm, test, or automation change.

Do not begin by opening `0.0.0.0/0` on every port. That can hide the diagnosis and create a security problem.

## Common interview questions

### Region versus Availability Zone

A Region is a separate geographic area. An Availability Zone is one or more discrete data centers with independent infrastructure inside a Region, connected with low-latency networking. Multi-AZ designs reduce the impact of a single-AZ failure; multi-Region designs address a larger failure domain but cost and operational complexity increase.

### EBS versus EFS versus S3

- EBS is block storage commonly attached to EC2. Think virtual disk and Availability Zone scope.
- EFS is a managed shared file system that multiple clients can mount.
- S3 is regional object storage accessed through APIs, not a normal block device.

Choose based on access pattern, sharing, latency, durability, protocol, and cost—not just “where files go.”

### Public versus private subnet

A public subnet has a route to an internet gateway. A private subnet does not. Private IPv4 instances can initiate internet access through a NAT gateway in a public subnet, but the NAT gateway does not accept unsolicited inbound connections for those instances.

### IAM role versus IAM user

A user is a long-term IAM identity. A role is assumed and provides temporary credentials. Workloads and cross-account access normally use roles. Human workforce access should generally use federation rather than creating many long-lived IAM users.

### Horizontal versus vertical scaling

Vertical scaling gives one node more resources. Horizontal scaling changes the number of nodes. Horizontal scaling usually improves fault tolerance but requires the application to distribute work and externalize shared state.

### RTO versus RPO

Recovery time objective is the targeted time to restore service. Recovery point objective is the acceptable amount of data loss measured in time. These business requirements drive backup frequency, replication, architecture, and cost.

### Shared responsibility model

AWS secures the infrastructure **of** the cloud. Customers secure their workloads **in** the cloud, with the dividing line changing by service type. With EC2, the customer manages more, including the guest OS and application. With a managed service, AWS manages more of the underlying stack, but the customer still owns identity, data, configuration, and appropriate use.

### Why Infrastructure as Code?

IaC makes infrastructure reviewable, repeatable, versioned, testable, and easier to recover. It reduces manual drift but does not automatically make an architecture safe; templates still require review, least privilege, testing, and controlled deployment.

## Architecture exercise: two-tier to three-tier web application

When explaining the repository architecture, say:

> I designed the entry point as an Application Load Balancer across two public subnets. The application tier is an Auto Scaling group across two private subnets, and only the load balancer security group can reach the application port. The data tier is isolated and accepts connections only from the application security group. EC2 uses an IAM role rather than stored keys. CloudWatch provides workload telemetry, CloudTrail provides API audit history, and Systems Manager is the preferred administrative path. I would validate backups and failover against the required RTO and RPO. For cost, I would right-size from observed utilization, scale on a workload metric, and use VPC endpoints where they improve security or avoid unnecessary NAT traffic.

Then state honestly whether it was designed, simulated, or deployed.

## STAR story prompts grounded in this repository

### LocalStack learning environment

- **Situation:** I wanted repeatable AWS practice without creating unnecessary live resources.
- **Task:** Build a safe local environment for service and CLI experiments.
- **Action:** I used Docker, LocalStack, and the AWS CLI; created an S3 workflow; verified operations; and documented troubleshooting and cleanup.
- **Result:** I created a reusable lab pattern and became more comfortable working through the CLI rather than only the console.

Add the exact problem you encountered and the exact verification command only after reviewing your terminal notes.

### EC2 first launch

- **Situation:** I needed to connect compute, network access, and key-based administration concepts.
- **Task:** Launch and understand the configuration of an EC2 instance.
- **Action:** I selected an AMI and instance type, configured a key pair and security group, launched the instance, and reviewed the access and lifecycle decisions.
- **Result:** I could explain how AMI, subnet, security group, key pair, storage, and instance state work together.

Do not add production scale, uptime, or cost savings unless you measured them.

## Questions to ask the interviewer

- How does the team define and measure reliability for its cloud workloads?
- How are infrastructure changes reviewed, tested, and promoted?
- What is the team's approach to observability and incident learning?
- Where is the environment on the spectrum from manual configuration to IaC?
- Which cloud skills would make someone effective in the first 90 days?

## Final review checklist

- [ ] Explain an EC2 launch from AMI through monitoring.
- [ ] Draw a VPC with public, private, and database subnets across two AZs.
- [ ] Walk one HTTPS request end to end.
- [ ] Explain roles and temporary credentials without saying “IAM is just Active Directory.”
- [ ] Compare security groups and NACLs.
- [ ] Compare EBS, EFS, and S3.
- [ ] Explain Auto Scaling separately from load balancing.
- [ ] Troubleshoot an unhealthy load balancer target.
- [ ] Explain CloudWatch versus CloudTrail.
- [ ] State RTO and RPO in plain language.
- [ ] Tell one specific troubleshooting story.
- [ ] Label every example accurately as completed, local, designed, or planned.

## Official references

- [Amazon EC2 instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)
- [VPC internet gateway basics](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [VPC route table concepts](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Security groups for a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [AWS Well-Architected Framework pillars](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html)
