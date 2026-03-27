# S3 Basics

## What is S3?
Amazon S3 (Simple Storage Service) is used to store files in the cloud.
Instead of saving files on a local machine or server, you can store them in S3 and access them over the internet.
S3 is designed to be highly durable and scalable, meaning it can store a large amount of data reliably.

## What is a Bucket?
A bucket is a container that holds your files in S3.
You create a bucket first, then upload objects (files) into it.
Each bucket name must be globally unique across AWS.

## What are Objects?
Objects are the actual files stored inside a bucket.
Each object includes:
- The file itself
- Metadata (information about the file)
- A unique key (its path or name in the bucket)
Example:
bucket-name/images/profile.jpg

## Key Idea
S3 is used for storage, not compute.
- EC2 = compute
- S3 = storage

## Common Use Cases
- Storing images for a website
- Backups and archives
- Application logs
- Hosting static websites
- Data storage for applications

## S3 and Permissions
S3 access is controlled using IAM and bucket policies.
IAM:
- Controls who can access S3
- Defines permissions for users and roles
Bucket Policies:
- Set rules directly on the bucket
- Control who can read or write data
By default, S3 buckets are private.

## S3 and EC2
S3 and EC2 are often used together.
- EC2 can upload files to S3
- EC2 can retrieve files from S3
- Applications running on EC2 store data in S3
Example:
A web app running on EC2 stores user uploads in S3.

## S3 and VPC
S3 is not inside your VPC, but it can still be accessed securely.
You can use:
- IAM roles for secure access
- VPC endpoints to connect privately without using the public internet

## How I Think About It
I think of S3 as the storage layer in AWS.
- EC2 handles the compute
- S3 stores the data
- IAM controls access
- VPC handles networking
S3 doesn’t run anything, it just stores and serves files when needed.

## My Understanding
S3 is a simple but powerful storage service.
It becomes really useful when combined with:
- EC2 for running applications
- IAM for secure access
- VPC for networking
It’s one of the core services used in most AWS setups.

## Future Topics
- S3 versioning
- Lifecycle policies
- Storage classes
- Static website hosting
