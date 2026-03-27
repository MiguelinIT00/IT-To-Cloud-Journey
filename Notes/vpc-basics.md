# VPC Basics

## What is a VPC?

A VPC (Virtual Private Cloud) is a private network inside AWS where you can launch and manage resources like EC2 instances.
It gives you control over networking, IP addresses, routing, and security.

## Key Idea
A VPC is the network layer in AWS.
- EC2 = compute
- S3 = storage
- IAM = permissions
- VPC = networking

## Why VPC Matters
A VPC helps organize and protect your AWS environment.
It lets you decide:
- Which resources can access the internet
- Which resources stay private
- How traffic moves between resources
- What network rules are allowed

## IP Address Range
When you create a VPC, you assign it a CIDR block.
A CIDR block is a range of IP addresses the VPC can use.
Example:
10.0.0.0/16
This gives the VPC a private address range for resources inside it.

## Subnets
A subnet is a smaller section of a VPC.
Resources like EC2 instances are launched into a subnet.
There are two common types:
- Public subnet
- Private subnet
A public subnet can allow access to the internet.
A private subnet is usually used for resources that should not be directly accessible from the internet.

## Route Tables
A route table controls where network traffic is directed.
It tells resources inside the subnet where to send traffic.
Example:
A route table can send internet-bound traffic to an internet gateway.

## Internet Gateway
An internet gateway allows communication between a VPC and the internet.
If a subnet is meant to be public, it usually needs:
- A route to the internet gateway
- Resources with public IP addresses

## Security Groups
A security group acts like a virtual firewall for a resource such as an EC2 instance.
It controls inbound and outbound traffic.
Example:
- Allow SSH on port 22 from your IP
- Allow HTTP on port 80
- Allow HTTPS on port 443

## Network ACLs
A Network ACL (Access Control List) is another layer of security for subnets.
It controls traffic entering and leaving the subnet.
Security groups apply at the resource level.
Network ACLs apply at the subnet level.

## VPC and EC2
EC2 instances run inside a VPC.
The VPC decides:
- What subnet the EC2 instance is in
- Whether it has internet access
- What traffic is allowed to reach it

## VPC and S3
S3 does not live inside your VPC, but it can still connect securely.
A VPC endpoint can allow private access from your VPC to S3 without sending traffic over the public internet.

## How I Think About It
I think of a VPC as the environment that holds everything together on the network side.
- EC2 lives inside it
- Subnets divide it up
- Route tables decide where traffic goes
- Security groups and ACLs help protect it
It is basically the network foundation for AWS resources.

## My Understanding
A VPC gives structure and security to AWS networking.
Instead of just launching resources into the cloud, a VPC lets you control how those resources communicate and whether they stay public or private.
It is one of the main building blocks of a secure AWS setup.

## Future Topics
- NAT Gateway
- VPC endpoints
- Peering
- DNS in a VPC
- Public vs private architecture
