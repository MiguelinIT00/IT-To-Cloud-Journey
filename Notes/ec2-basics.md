# Amazon EC2 Deep Dive

## What EC2 provides

Amazon Elastic Compute Cloud provides virtual compute instances. EC2 gives control over the operating system and software stack, which also means the customer owns patching, hardening, application configuration, and workload monitoring.

## Decisions in an instance launch

### AMI

An Amazon Machine Image supplies the boot volume contents and launch permissions. AMIs can be AWS-provided, Marketplace-based, community-based, or custom. A controlled AMI pipeline helps make hosts repeatable.

### Instance type

The type describes a hardware profile: virtual CPU, memory, networking, storage behavior, and possibly accelerators. Families are optimized for general purpose, compute, memory, storage, or accelerated workloads.

Choose using measurements. A CPU alarm alone cannot reveal memory pressure unless memory metrics are collected from the guest OS.

### Network placement

An instance launches into one subnet in one Availability Zone. Its network interface receives a private address. Public IPv4 reachability additionally requires a public IPv4 address/Elastic IP, correct routing, and traffic allowed by network controls.

### Security group

Security groups are stateful allow-list firewalls associated with network interfaces. Start with no unnecessary inbound rules. Reference another security group when the intended source is another application tier.

### Storage

- **EBS:** persistent block storage, normally scoped to one AZ and attached like a disk.
- **Instance store:** temporary storage physically associated with the host; data does not survive all lifecycle events.
- **S3:** object storage accessed through APIs; not a replacement for a normal boot disk.
- **EFS:** managed shared file storage mountable by multiple clients.

### IAM role

Attach a least-privilege role through an instance profile. Applications then receive temporary credentials through the instance metadata service instead of storing access keys on disk.

### Bootstrap and management

User data can bootstrap a host on launch, but long scripts can become difficult to test and observe. Mature environments combine a trusted image, short bootstrap logic, configuration management, and an immutable or replaceable-instance approach.

Prefer Systems Manager Session Manager for administration when possible so inbound SSH/RDP does not have to be exposed.

## Lifecycle

| Action | Compute billing | Host placement | Private IPv4 | Public IPv4 | EBS data |
|---|---|---|---|---|---|
| Reboot | Continues | Normally same host | Preserved | Preserved | Preserved |
| Stop/start | Stops while fully stopped; other resources can still cost money | May change | Preserved | Usually changes unless Elastic IP | Preserved |
| Hibernate | RAM state is saved when supported/configured | May change | Preserved | Usually changes unless Elastic IP | Preserved |
| Terminate | Instance compute billing ends | Instance removed | Released | Released | Depends on `DeleteOnTermination` |

Always verify volume, snapshot, IP, and dependent-resource behavior before changing lifecycle state.

## Purchase options

- **On-Demand:** flexible, no long commitment; useful for uncertain or short-lived demand.
- **Savings Plans:** commitment based; appropriate after understanding steady usage.
- **Reserved Instances:** billing benefit with attributes and scope that vary by offering.
- **Spot Instances:** discounted spare capacity that can be interrupted; design the workload to tolerate interruption.
- **Dedicated Hosts/Instances:** stronger tenancy control for specific compliance or licensing cases at higher cost.

## EC2 operational checklist

- Use an approved, patched AMI.
- Require IMDSv2 where compatible.
- Attach a least-privilege IAM role; do not store access keys.
- Encrypt EBS volumes with the appropriate KMS key.
- Limit security-group rules to required sources and ports.
- Send system/application logs to a durable central location.
- Collect workload metrics, not only default host metrics.
- Back up or snapshot state according to RPO.
- Tag owner, environment, application, data classification, and cost center.
- Define patching, replacement, recovery, and termination-protection expectations.

## Common failure scenarios

### Cannot connect with SSH

Check instance state and status checks, correct address, route path, public/Elastic IP, security-group source CIDR, NACL rules, host firewall, SSH service, username, and key. Prefer Session Manager when configured.

### Website times out

Check DNS, load balancer/target health, route path, security groups, NACLs, process listener, host firewall, application logs, and dependency health.

### Instance passes status checks but application fails

EC2 status checks do not understand application correctness. Check the process, port, disk/memory pressure, configuration, secrets, downstream services, and application-specific health endpoint.

## Official references

- [EC2 instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)
- [Instance types](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html)
- [Security groups](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html)
- [IAM roles for applications on EC2](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2.html)
