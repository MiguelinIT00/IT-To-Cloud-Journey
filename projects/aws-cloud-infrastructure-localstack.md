# AWS Cloud Infrastructure Learning Project with LocalStack

**Status:** Active local learning project. Completed work currently covers the environment and S3 CLI lifecycle; later components are planned.

## Why I built it

I wanted a low-risk, repeatable environment for learning cloud APIs and troubleshooting. LocalStack lets me experiment locally before deciding when real AWS validation is necessary.

## Current architecture

```text
PowerShell / AWS CLI
        |
LocalStack endpoint in Docker
        |
S3 bucket and objects (completed)

Planned: S3 event -> Lambda -> log/output
Planned: IaC template -> repeatable environment
```

## Completed capabilities

- Local containerized AWS emulator.
- AWS CLI calls directed at an explicit local endpoint.
- S3 bucket and object create/read/delete workflow.
- Verification after each mutation.
- Resource cleanup.
- Documentation of the difference between emulation and AWS validation.

## Planned iterations

1. S3 event triggers a Lambda function.
2. Function writes structured logs and an output object.
3. Negative test proves the workload cannot access an out-of-scope resource.
4. IaC recreates the project from an empty environment.
5. Automated tests verify expected resources and behavior.
6. A controlled AWS sandbox validates the parts LocalStack cannot prove.

## Engineering questions I am practicing

- How do services authenticate and authorize each call?
- Where does state live and how is it recovered?
- What evidence proves an operation succeeded?
- How does the application behave when an event is delivered more than once?
- What logs and metrics would an operator need?
- How can the environment be recreated and removed safely?
- Which behaviors are emulator-specific and need AWS validation?

## Interview summary

> I created a repeatable local environment with Docker, LocalStack, and the AWS CLI so I could practice AWS-style resource workflows without leaving chargeable resources running. I completed the S3 object lifecycle and documented verification and cleanup. The project taught me to make the endpoint and caller explicit and to separate local emulation from evidence gathered in AWS. My next step is an event-driven S3-to-Lambda workflow defined as code, with positive and negative tests.

That summary describes the work completed without presenting the emulator as production AWS.
