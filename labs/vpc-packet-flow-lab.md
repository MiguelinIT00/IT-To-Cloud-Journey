# Study Lab: Trace a Packet Through a VPC

**Status:** Ready to execute. The design and verification steps are documented; completion boxes should remain open until commands are run.

## Objective

Build or inspect a two-AZ VPC and prove why a subnet is public or private by tracing routes and security controls.

## Learning topology

```text
VPC 10.20.0.0/16

AZ A                              AZ B
10.20.0.0/24 public               10.20.1.0/24 public
  ALB / NAT gateway                 ALB / NAT gateway

10.20.10.0/24 private app         10.20.11.0/24 private app
  EC2 target                         EC2 target

10.20.20.0/24 isolated DB         10.20.21.0/24 isolated DB
```

## Route expectations

| Subnet tier | Default IPv4 route | Purpose |
|---|---|---|
| Public | `0.0.0.0/0 -> internet gateway` | Public entry points and NAT gateway |
| Private app | `0.0.0.0/0 -> same-AZ NAT gateway` | Outbound updates/API access without direct inbound internet path |
| Isolated DB | No general internet default route | Data tier communicates only through intended private routes |

## Security-group expectations

- ALB: allow HTTPS from intended client CIDRs; forward only to the app port.
- App: allow the app port from the ALB security group, not from the internet.
- Database: allow the database port from the app security group only.

## Verification workflow

```powershell
aws sts get-caller-identity
aws ec2 describe-vpcs --query "Vpcs[].{Id:VpcId,Cidr:CidrBlock}" --output table
aws ec2 describe-subnets --query "Subnets[].{Id:SubnetId,AZ:AvailabilityZone,Cidr:CidrBlock,Vpc:VpcId}" --output table
aws ec2 describe-route-tables --output json
aws ec2 describe-security-groups --output json
```

Do not publish account IDs, public IPs, or other sensitive environment details in the repository.

For one flow, write down:

1. source address/security group;
2. destination address/security group;
3. DNS result;
4. source route-table match;
5. gateway/endpoint/transit target;
6. inbound filter result;
7. process listener;
8. return route and stateless NACL requirements.

## Failure experiments

- [ ] Remove the app SG rule from the ALB SG and record the target-health symptom.
- [ ] Restore the SG rule and verify recovery.
- [ ] Point a private route at the wrong target and observe the connectivity symptom.
- [ ] Restore the route and verify recovery.
- [ ] Stop the app process while leaving the instance running and compare EC2 checks with application health.
- [ ] Enable and inspect VPC Flow Logs for accepted/rejected metadata.

## Completion evidence

- [ ] Architecture diagram matches deployed resource IDs.
- [ ] Route-table output proves public/private classification.
- [ ] Health check succeeds before failure tests.
- [ ] Each failure has a symptom, evidence, root cause, fix, and verification.
- [ ] Chargeable resources are removed or intentionally retained with an owner.

## Cleanup

Inventory and remove test load balancers, NAT gateways, Elastic IPs, EC2 instances, volumes/snapshots, endpoints, logs, and the VPC when no longer needed. Dependency order matters; confirm every deletion in the console or CLI.
