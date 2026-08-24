# Completed Lab: First EC2 Launch

## Objective

Launch an EC2 instance and understand the configuration decisions connecting compute, storage, identity, and networking.

## Work completed

- Opened the EC2 launch workflow in the AWS Management Console.
- Selected an Amazon Linux or Windows AMI for the exercise.
- Selected a small learning instance type.
- Created or selected a key pair.
- Configured a security group for administrative access.
- Launched the instance and reviewed its lifecycle controls.

## What each choice means

| Choice | Why it matters |
|---|---|
| AMI | Defines the starting operating system and software image |
| Instance type | Defines the compute/memory/network hardware profile |
| Subnet | Places the instance in one AZ and route context |
| Security group | Allows only required network flows |
| Key pair | Supports key-based login when SSH/RDP is used |
| EBS volume | Provides persistent block storage |
| IAM role | Gives applications temporary AWS permissions without stored keys |

## Security lesson

Administrative ports should never be opened broadly as a shortcut. Restrict SSH/RDP to a trusted source, or prefer Systems Manager Session Manager when the environment supports it. Application access and administrative access should use separate rules and paths.

## What I learned

- An instance is only one part of the system; access depends on routes, addressing, security groups, host configuration, and credentials.
- EC2 status checks show infrastructure/OS reachability signals, not full application health.
- Starting, stopping, rebooting, and terminating have different effects on billing, addressing, host placement, and data.
- A cleanup plan is part of the lab, not an afterthought.

## Next iteration evidence checklist

- [ ] Record the chosen AMI ID and instance type.
- [ ] Record the VPC/subnet design without exposing sensitive account data.
- [ ] Connect using Session Manager or a tightly restricted administrative path.
- [ ] Install and verify a simple web service.
- [ ] Attach a least-privilege IAM role and test an allowed and denied API action.
- [ ] Create a CloudWatch alarm and collect application logs.
- [ ] Stop/start and record the observed IP/storage behavior.
- [ ] Terminate the test resources and verify cleanup.

This file distinguishes the completed first launch from the deeper validation steps still to be performed.
