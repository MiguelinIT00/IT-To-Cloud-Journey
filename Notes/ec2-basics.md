# EC2 Basics

## What is an EC2 Instance?

An Amazon EC2 instance is basically a virtual server in AWS.

Instead of dealing with physical hardware, you can launch a server in the cloud that you can start, stop, resize, and configure whenever you need it.

It’s like having a computer that lives in AWS and runs whatever you want.

## What EC2 is Used For

Some common things you can do with EC2:

- Host websites or web apps  

- Run backend services  

- Test software  

- Run scripts or automation  

- Create cloud lab environments  

## Key Idea

EC2 gives you compute power on demand.

You don’t need to buy or maintain hardware. You just spin up a server when you need it.

---

## EC2 in the AWS Ecosystem (Additional Notes)

These notes expand on how EC2 interacts with other core AWS services.

EC2 is not just a standalone server. It works together with other AWS services to build a full environment.

---

### EC2 + IAM (Permissions)

IAM controls access and permissions in AWS.

- IAM users or roles control who can manage EC2  

- EC2 instances can also have an IAM role attached  

Why this matters:

Instead of storing credentials on the server, the EC2 instance can securely access other AWS services using its IAM role.

Example:

An EC2 instance can pull files from S3 without storing access keys.

---

### EC2 + S3 (Storage)

S3 is used to store files like images, backups, logs, and other data.

EC2 and S3 usually work together:

- EC2 processes or generates data  

- S3 stores that data  

Examples:

- A web app on EC2 uploads images to S3  

- EC2 reads config files from S3  

- Logs and backups from EC2 are stored in S3  

Simple way to think about it:

- EC2 = compute  

- S3 = storage  

---

### EC2 + VPC (Networking)

Every EC2 instance runs inside a VPC (Virtual Private Cloud).

The VPC is basically the network your EC2 lives in.

It controls things like:

- IP addresses  

- Internet access  

- Security rules  

Key parts:

Subnets:

- Public subnet → has internet access  

- Private subnet → no direct internet access  

Security Groups:

- Act like a firewall for EC2  

- Control what traffic is allowed in and out  

Example:

Only allow SSH (port 22) from your IP.

---

### How I Think About It

I think of EC2 as the center of everything.

It runs the application, and then:

- IAM controls what it’s allowed to access  

- S3 is where data gets stored or retrieved  

- VPC is the network it lives in  

All of these pieces work together rather than in a strict step-by-step flow.

---

## My Understanding

EC2 is the compute layer in AWS, but it becomes powerful when combined with other services.

- IAM handles security and permissions  

- S3 handles storage  

- VPC handles networking  

So instead of thinking of EC2 as just a virtual machine, I see it as part of a bigger system where everything connects together.
 
