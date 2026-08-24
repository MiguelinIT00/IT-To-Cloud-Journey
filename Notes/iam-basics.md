# AWS IAM Deep Dive

## Authentication and authorization

- **Authentication:** prove the identity of a person or workload.
- **Authorization:** decide whether that principal may perform an action on a resource in the current context.

IAM is global rather than Region-scoped. It controls access to AWS APIs; it is not a direct replacement for a full corporate directory, device-management platform, or application user database.

## Principals and access patterns

### Human access

Prefer workforce federation and temporary access, with MFA. IAM Identity Center can connect workforce identities to AWS accounts and applications. Avoid creating long-lived IAM users for every employee when federation is available.

### Workload access

Use roles for EC2, Lambda, containers, AWS services, and cross-account access. A role is assumed and provides temporary credentials. Avoid embedding access keys in code, images, environment files, or source control.

### Root user

Protect root credentials, require MFA, do not create root access keys, and use root only for tasks that require it.

## Policy anatomy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadStudyObjects",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::example-study-bucket/materials/*"
    }
  ]
}
```

A statement commonly contains:

- `Effect`: `Allow` or `Deny`;
- `Action` or `NotAction`;
- `Resource` or `NotResource`;
- optional `Condition` values;
- `Principal` in policy types where the principal is specified, such as a resource-based policy or role trust policy.

The example is a study pattern; the bucket name must be changed before use.

## Identity policy versus trust policy

An EC2 role has two different questions:

1. **Who may assume the role?** The trust policy normally allows the EC2 service principal.
2. **What may the role do after assumption?** Permissions policies allow specific API actions on specific resources.

Confusing these is a common troubleshooting issue.

## Simplified policy evaluation

1. Start with an implicit deny.
2. An applicable allow can grant access.
3. An applicable explicit deny overrides an allow.
4. Effective permissions can be intersected or limited by identity policies, resource policies, permissions boundaries, session policies, organization service control policies, endpoint policies, and service-specific rules.

When access is denied, identify the principal ARN, action, resource ARN, Region/account, policy types, and condition context. Do not immediately attach administrator access.

## Least privilege workflow

1. Define the workload operation in plain language.
2. Map it to required API actions and resources.
3. Restrict resource ARNs and use conditions where practical.
4. Test both intended access and intended denial.
5. Review CloudTrail/access information and narrow permissions.
6. Set an owner and review date.

## IAM and Active Directory

Both involve identities, groups, authentication, and authorization, but they operate at different layers:

| AWS IAM | Microsoft Active Directory |
|---|---|
| Authorizes AWS API/resource access | Directory and domain services for users, computers, applications, and enterprise resources |
| Policies, roles, and temporary AWS credentials | Users, computers, groups, Kerberos/LDAP, Group Policy, domain trust |
| Not a domain join or device policy system | Commonly supports domain join and centralized Windows management |

Federation connects identity systems; it is more accurate than saying IAM “is AWS Active Directory.” AWS also provides directory-focused services when that is the requirement.

## Interview checks

- Explain why an EC2 instance role is safer than a stored access key.
- Distinguish a role trust policy from its permissions policy.
- Explain explicit deny and implicit deny.
- Describe how to troubleshoot `AccessDenied` without granting broad access.
- Give an example of resource restriction and a useful condition.

## Official references

- [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
- [Policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)
- [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
