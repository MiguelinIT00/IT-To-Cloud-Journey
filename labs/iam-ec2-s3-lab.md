# Study Lab: Least-Privilege EC2 Access to S3

**Status:** Ready to execute in an authorized test account. LocalStack can exercise the API workflow, while a real AWS account is needed to validate AWS IAM behavior accurately.

## Objective

Allow an application on EC2 to read one S3 prefix using temporary role credentials, without storing an access key.

## Design

```text
EC2 instance
  -> instance profile
  -> IAM role with s3:GetObject
  -> one bucket prefix: materials/*
  -> optional S3 gateway endpoint
```

## Example permissions policy

Replace the example bucket name before use.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ListOnlyStudyPrefix",
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::example-study-bucket",
      "Condition": {
        "StringLike": {
          "s3:prefix": ["materials/*"]
        }
      }
    },
    {
      "Sid": "ReadOnlyStudyObjects",
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-study-bucket/materials/*"
    }
  ]
}
```

The EC2 service must be allowed in the role's trust policy. The trust policy controls role assumption; the permissions policy controls what the assumed role may do.

## Test matrix

| Test | Expected result |
|---|---|
| Identify caller with STS | Assumed-role ARN, not an IAM user's stored key |
| List the allowed prefix | Allowed |
| Read an object in `materials/` | Allowed |
| Upload an object | Denied |
| Read an object outside the prefix | Denied |
| List another bucket | Denied |

## Verification commands

```powershell
aws sts get-caller-identity
aws s3api list-objects-v2 --bucket example-study-bucket --prefix materials/
aws s3 cp s3://example-study-bucket/materials/example.txt -
aws s3 cp .\should-not-upload.txt s3://example-study-bucket/materials/should-not-upload.txt
```

The final command is supposed to fail. A denied test is useful evidence that least privilege is working.

## Troubleshooting `AccessDenied`

1. Record caller, action, resource, and error request ID.
2. Check whether the role was actually attached and assumed.
3. Check identity and bucket policies.
4. Check permissions boundary, organization SCP, session policy, and endpoint policy when present.
5. Check KMS key policy/grants if the object uses SSE-KMS.
6. Inspect CloudTrail events in the authorized environment.

## Security and cleanup

- Never paste credentials or full account identifiers into this repository.
- Remove the test role, instance profile, bucket objects, and bucket when finished.
- Retain only sanitized policy examples and observations.

## Completion evidence

- [ ] Role is assumed through the EC2 instance profile.
- [ ] Allowed read succeeds.
- [ ] Unauthorized write and out-of-scope read fail.
- [ ] CloudTrail evidence is reviewed.
- [ ] Resources are cleaned up.
