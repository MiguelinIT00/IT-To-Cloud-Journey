# AWS Storage: S3, EBS, and EFS

## Amazon S3

S3 is regional object storage. Data is stored as objects in buckets and accessed through APIs. An object has data, a key, and metadata. S3 is not a normal block disk and does not run application code.

### Core controls

- Block Public Access settings help prevent unintended public exposure.
- IAM and bucket policies authorize access.
- Encryption protects data at rest; TLS protects data in transit.
- Versioning helps recover from overwrite or deletion but can increase storage use.
- Lifecycle rules transition or expire objects based on requirements.
- Object Lock supports write-once-read-many retention use cases when configured.
- Access logging and CloudTrail data events can provide different forms of visibility.

### Storage classes

Select a class based on access frequency, retrieval-time needs, resilience pattern, minimum duration, retrieval cost, and monitoring/automation needs. Lifecycle policies are useful only when the data's access and retention requirements are understood.

### S3 consistency and design

S3 provides strong read-after-write consistency for object PUT and DELETE operations. Applications still need to handle retries, idempotency, authorization, naming, and concurrent business workflows correctly.

## S3, EBS, and EFS comparison

| Characteristic | S3 | EBS | EFS |
|---|---|---|---|
| Storage model | Object | Block | File |
| Common access | HTTPS API | Attached volume | NFS mount |
| Scope concept | Regional service | Volume in an AZ | Regional file system with mount targets |
| Sharing | Many API clients | Attachment behavior depends on volume/type and design | Shared by multiple clients |
| Typical use | Backups, logs, media, data lake, static assets | Boot/data disk, database volume | Shared Linux content or home directories |

## Secure EC2-to-S3 pattern

1. Keep the bucket private and enable Block Public Access.
2. Attach an IAM role to EC2 through an instance profile.
3. Grant only required actions on the required bucket/prefix.
4. Consider an S3 gateway VPC endpoint for private routing.
5. Restrict policies further when the access path and account design justify it.
6. Enable versioning, encryption, logging, and lifecycle controls based on requirements.
7. Test allowed and denied operations.

## Troubleshooting S3 `AccessDenied`

- Confirm the caller with `aws sts get-caller-identity`.
- Confirm the exact bucket, key, action, account, and Region.
- Check the identity policy and bucket/access-point policy.
- Check for explicit denies in Block Public Access behavior, permissions boundaries, SCPs, endpoint policies, KMS key policies, or conditions.
- If using SSE-KMS, verify both S3 and KMS permissions.
- Use CloudTrail events and policy-analysis tools rather than guessing.

## Official references

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [S3 security best practices](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)
- [Amazon EBS](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html)
- [Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)
