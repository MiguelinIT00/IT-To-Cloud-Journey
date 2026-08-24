# Infrastructure as Code Study Templates

These templates are reviewable learning artifacts. They have not been deployed as part of this update.

## EC2 study stack

[`ec2-study-stack.json`](ec2-study-stack.json) creates:

- one VPC and public subnet;
- an internet gateway and public route;
- a security group with no inbound rules;
- an EC2 role for Systems Manager;
- one Amazon Linux 2023 EC2 instance with detailed monitoring;
- a simple CPU CloudWatch alarm.

The instance uses Session Manager instead of opening SSH. User data installs a simple web service for local-on-instance verification, but the service is intentionally not exposed to the internet.

## Safety and cost

Creating a CloudFormation change set and reviewing it is safer than immediately deploying. Deployment creates a chargeable EC2 instance and related telemetry/data transfer usage. Check current pricing, account permissions, Region, quotas, and organizational policies first.

Delete the stack after the authorized lab and verify that its resources are gone. CloudFormation cannot delete resources created outside the stack, and retained or separately created resources may continue to cost money.

## Validation workflow

```powershell
aws cloudformation validate-template `
  --template-body file://templates/ec2-study-stack.json

aws cloudformation create-change-set `
  --stack-name ec2-study `
  --change-set-name review-only `
  --change-set-type CREATE `
  --template-body file://templates/ec2-study-stack.json `
  --capabilities CAPABILITY_NAMED_IAM
```

The first command validates the template through AWS. The second creates a reviewable change set but does not execute it. Do not execute the change set without authorization and a cleanup plan.
