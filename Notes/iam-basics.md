# IAM Basics

## What is IAM?

IAM (Identity and Access Management) is used to control who can access AWS resources and what actions they can perform.
It helps you manage users, permissions, and security across your AWS environment.

## Key Idea
IAM is the security layer of AWS.
- EC2 = compute  
- S3 = storage  
- IAM = permissions 

## IAM Users
An IAM user represents a person or service that interacts with AWS.
Each user can have:
- A username and password (for AWS Console access)
- Access keys (for programmatic access)
Example:
A developer logging into AWS to manage EC2 instances.

## IAM Roles
An IAM role is a set of permissions that can be assigned to AWS services.
Roles are commonly used by:
- EC2 instances  
- Lambda functions  
- Other AWS services  
Why this matters:
Instead of storing credentials, AWS services can assume a role to get temporary permissions.
Example:
An EC2 instance uses a role to access S3 securely.

## IAM Policies
Policies define what actions are allowed or denied.
They are written in JSON and attached to users, groups, or roles.
Policies control:
- What actions can be performed  
- Which resources can be accessed  
Example:
Allow reading objects from an S3 bucket.

## IAM Groups
Groups are collections of users.
Instead of assigning permissions to each user individually, you assign them to a group.
Example:
A “Developers” group with access to EC2 and S3.

## IAM and EC2
IAM controls who can manage EC2 instances and what EC2 instances can access.
- Users can be given permission to start/stop EC2  
- EC2 instances can use IAM roles to access other AWS services  

## IAM and S3
IAM controls access to S3 buckets and objects.
- Users can be allowed or denied access to specific buckets  
- EC2 can use IAM roles to read/write to S3 

## How I Think About It
I think of IAM as the gatekeeper of AWS.
- It decides who gets access  
- What they can do  
- And what they cannot do  
Nothing in AWS should be accessed without IAM controlling it.

## My Understanding
IAM is critical for security in AWS.
It works together with:
- EC2 for compute  
- S3 for storage  
Without IAM, there would be no control over access, which would make the environment insecure.

## Future Topics
- IAM policy structure (JSON)  
- Least privilege principle  
- Multi-factor authentication (MFA)  
- Role assumption
