# Observability and Cloud Troubleshooting

## Monitoring versus observability

Monitoring tells operators that a known condition occurred. Observability helps operators ask new questions about system behavior using telemetry and context.

Useful signals:

- **Metrics:** numeric trends and thresholds.
- **Logs:** detailed event records from applications, operating systems, and services.
- **Traces:** a request's path across components.
- **Events:** state changes and control-plane activity.

## AWS operations map

| Need | Service/tool |
|---|---|
| Resource and custom metrics, logs, alarms, dashboards | CloudWatch |
| AWS API activity and audit investigation | CloudTrail |
| Configuration history and rule evaluation | AWS Config |
| Network flow metadata | VPC Flow Logs |
| Remote administration, patching, inventory, automation | Systems Manager |
| Application request tracing | AWS X-Ray / OpenTelemetry tooling |
| Health events affecting AWS services/resources | AWS Health |

CloudWatch and CloudTrail are not interchangeable: one focuses on operational telemetry, while the other records supported API activity.

## Golden signals

- Latency
- Traffic
- Errors
- Saturation

Add business-level signals such as successful checkout or job completion. Infrastructure may look healthy while the user outcome is failing.

## Incident method

1. Stabilize safety and user impact.
2. State the symptom precisely: who, what, where, when, and scope.
3. Build a timeline and identify recent changes.
4. Follow the request path and find the first failing boundary.
5. Form one testable hypothesis.
6. Gather the smallest evidence that confirms or rejects it.
7. Apply the lowest-risk reversible mitigation.
8. Verify user recovery and watch for regression.
9. Determine root cause and contributing conditions.
10. Create prevention work: alarm, test, runbook, guardrail, automation, or design change.

## Example: high latency behind an ALB

Evidence to inspect:

- ALB request count, target response time, status-code categories, and rejected connections;
- healthy host count and target-group reason codes;
- application latency and error logs;
- EC2 CPU, collected memory/disk metrics, and network behavior;
- database connections, locks, CPU, storage latency, and slow queries;
- deployment/configuration changes;
- scaling activity and instance warm-up.

Possible causes include overloaded targets, slow database calls, a bad deployment, exhausted connection pools, uneven requests, or dependencies. Scaling out may reduce symptoms but does not replace root-cause analysis.

## Runbook structure

Every runbook should include:

- trigger and user-visible symptom;
- prerequisites and access level;
- diagnostic commands with expected results;
- safe mitigation and rollback;
- verification from the user's perspective;
- escalation owner;
- cleanup and evidence-retention notes.

## Official references

- [Amazon CloudWatch concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)
- [What is AWS CloudTrail?](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
- [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)
