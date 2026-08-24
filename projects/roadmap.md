# Cloud Engineering Project Roadmap

## Phase 1: Foundations

- [x] Document AWS core concepts.
- [x] Complete an initial EC2 launch.
- [x] Build a LocalStack/AWS CLI environment.
- [x] Complete a local S3 lifecycle lab.

## Phase 2: Depth and verification

- [ ] Complete the EC2 evidence checklist with monitoring and cleanup.
- [ ] Trace public/private subnet packet flow.
- [ ] Prove least-privilege EC2-to-S3 access with allow and deny tests.
- [ ] Build S3-to-Lambda event processing locally.
- [ ] Add repeatable IaC and automated validation.

## Phase 3: Architecture and operations

- [ ] Deploy an authorized nonproduction version of the multi-AZ web design.
- [ ] Add ALB, Auto Scaling, central logs, alarms, and safe deployment.
- [ ] Test instance failure and record recovery time.
- [ ] Perform a database backup restore/failover exercise.
- [ ] Review security, reliability, performance, cost, operations, and sustainability.

## Evidence standard

Every completed item should include:

- date and environment (LocalStack, AWS sandbox, or design-only);
- architecture or request-flow diagram;
- sanitized commands/configuration;
- expected and actual verification;
- at least one failure or denied test;
- cleanup result;
- what I would change next time.

Check an item only after its evidence exists.
