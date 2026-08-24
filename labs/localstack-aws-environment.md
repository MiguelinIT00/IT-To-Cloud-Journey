# Completed Foundation: Local AWS Development Environment

## Purpose

I added LocalStack, Docker Desktop, and the AWS CLI to create a repeatable local cloud-learning environment. It lets me practice service APIs, automation, failure testing, and cleanup before deciding whether a lab needs real AWS behavior.

## Tooling

- LocalStack
- Docker Desktop
- AWS CLI
- PowerShell
- Git and GitHub

## What this environment is good for

- Learning AWS CLI command structure.
- Practicing resource create/read/update/delete workflows.
- Developing scripts and IaC quickly.
- Testing application integration with supported local service emulation.
- Intentionally breaking configuration and documenting diagnosis.

## Important boundary

LocalStack is an emulator, not an AWS account. It is useful for learning and development, but it does not prove real AWS IAM evaluation, service quotas, managed control-plane behavior, availability, latency, pricing, or every service feature. Labs must say which environment produced the evidence.

## Repeatable workflow

1. Start the containerized environment.
2. Verify the LocalStack health endpoint.
3. Point the AWS CLI at the local endpoint using test-only credentials.
4. Create resources through CLI or IaC.
5. Run positive and negative tests.
6. Capture sanitized commands, results, and errors.
7. Delete resources and stop the environment.

## Current completed exercise

The S3 lab covers bucket creation, object upload/list/download/delete, and cleanup through the CLI. The next iteration is an event-driven S3-to-Lambda workflow, followed by IaC for repeatability.

**Build → Break → Troubleshoot → Fix → Document → Repeat.**
