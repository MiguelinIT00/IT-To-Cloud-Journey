# AWS Lambda Fundamentals

Lambda runs code in response to invocations without the customer managing servers. AWS operates the underlying compute fleet; the customer still owns function code, IAM permissions, configuration, dependencies, data protection, observability, and failure handling.

## Execution model

- Functions run in execution environments that may be reused but should not be treated as durable state.
- The handler receives an event and runtime context.
- Memory configuration also affects available CPU and other resources.
- Each invocation has a maximum duration; long-running jobs need another compute pattern or decomposition.
- Scaling is managed, but concurrency, downstream capacity, quotas, and cost still require design.

## Common event sources

- API Gateway or function URLs for HTTP.
- S3 notifications for object events.
- EventBridge for events and schedules.
- SQS for buffered asynchronous processing.
- DynamoDB Streams or Kinesis for stream records.

## Reliability patterns

- Assume events may be delivered more than once and design idempotent processing.
- Use retries with bounded backoff and understand the event source's retry behavior.
- Configure dead-letter or failure destinations where supported and useful.
- Protect downstream systems with reserved concurrency, queues, and backpressure.
- Record correlation identifiers and meaningful outcome metrics.

## Security

- Give the execution role only required actions/resources.
- Store secrets in an appropriate managed service, not in source code.
- Validate untrusted event input.
- Keep dependencies patched and deployment artifacts controlled.
- Use VPC attachment only when the function needs private VPC resources; understand the networking and cold-start/operational implications.

## Lambda versus EC2

Choose Lambda for event-driven, short-lived, horizontally scalable work that fits its execution model. Choose EC2 when the workload needs OS control, special agents/drivers, long-running processes, predictable dedicated capacity, or software that does not fit the function model. Containers and managed compute provide additional options.

## Planned local project

S3 object creation will invoke a Lambda function that writes a derived object and structured log. The lab will test duplicate delivery, invalid input, denied access, and cleanup. Until executed, it remains planned.

## Official reference

- [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
