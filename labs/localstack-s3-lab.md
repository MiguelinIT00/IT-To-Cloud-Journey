# Completed Lab: LocalStack S3 with the AWS CLI

## Objective

Practice the S3 resource lifecycle from the command line in a local emulated AWS environment.

## Environment

- LocalStack and Docker Desktop
- AWS CLI
- PowerShell
- Test-only local credentials

## Tasks completed

- Started and verified the LocalStack environment.
- Created an S3 bucket through the CLI.
- Listed buckets.
- Uploaded an object.
- Listed and downloaded the object.
- Deleted the object and bucket.

## Example command pattern

These commands use example names and the local endpoint. They are not evidence of a currently running environment.

```powershell
$endpoint = "http://localhost:4566"
$bucket = "cloud-study-local"

aws --endpoint-url $endpoint s3api create-bucket --bucket $bucket
aws --endpoint-url $endpoint s3 ls
aws --endpoint-url $endpoint s3 cp .\example.txt "s3://$bucket/example.txt"
aws --endpoint-url $endpoint s3api list-objects-v2 --bucket $bucket
aws --endpoint-url $endpoint s3 cp "s3://$bucket/example.txt" .\downloaded-example.txt
aws --endpoint-url $endpoint s3 rm "s3://$bucket/example.txt"
aws --endpoint-url $endpoint s3api delete-bucket --bucket $bucket
```

## Verification mindset

Each mutation should have a read-after step:

- After create: list or describe the bucket.
- After upload: list/head the object and compare expected content.
- After delete: verify the object or bucket is absent.
- On failure: capture the exact caller, endpoint, command, and error.

## What I learned

- The AWS CLI exposes infrastructure operations directly and makes them repeatable.
- Endpoint configuration matters; a command pointed at AWS is not the same as a command pointed at LocalStack.
- Bucket and object operations are separate API concepts.
- Cleanup and negative tests are part of the lab.
- Local emulation does not validate all real AWS permissions, durability, availability, or service behavior.

## Next experiment

- [ ] Configure an S3 event notification.
- [ ] Trigger a Lambda function in LocalStack.
- [ ] Make processing idempotent.
- [ ] Inspect logs and retry behavior.
- [ ] Define the stack with IaC and recreate it from zero.
