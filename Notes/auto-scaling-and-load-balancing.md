# Elastic Load Balancing and EC2 Auto Scaling

## Different jobs

- A **load balancer** distributes connections or requests to healthy targets.
- An **Auto Scaling group (ASG)** maintains and changes EC2 capacity.
- A **target group** registers destinations and defines health-check behavior.

They often work together, but none of them automatically makes an application stateless, a database scalable, or a deployment safe.

## Load balancer families

| Type | Layer/use | Typical decision |
|---|---|---|
| Application Load Balancer | HTTP/HTTPS, Layer 7 | Host/path routing, web apps, services |
| Network Load Balancer | TCP/UDP/TLS, Layer 4 | Very high performance, static IP needs, non-HTTP protocols |
| Gateway Load Balancer | Network appliance insertion | Firewalls and inspection appliances |
| Classic Load Balancer | Previous generation | Maintain legacy workloads; use current families for new designs |

## Health checks

A useful health check is cheap, fast, and representative. It should detect whether the target can serve traffic without causing a dependency failure to remove every target at once. Separate shallow liveness from deeper readiness when the platform and application support it.

When a target is unhealthy, check:

- target registration and correct port;
- load balancer SG egress and target SG ingress;
- health-check protocol, port, path, host behavior, success codes, timeout, and thresholds;
- process listener and host firewall;
- application logs and dependency health.

## Auto Scaling components

- Launch template: AMI, instance type, networking, role, storage, and bootstrap settings.
- Minimum capacity: lower boundary.
- Desired capacity: capacity the group attempts to maintain.
- Maximum capacity: upper boundary.
- Health checks: EC2 and optionally load balancer health.
- Scaling policies: rules or targets that change desired capacity.

## Scaling approaches

- **Target tracking:** keep a metric near a target, similar to a thermostat.
- **Step scaling:** change capacity by different amounts depending on alarm severity.
- **Scheduled scaling:** adjust for predictable time-based demand.
- **Predictive scaling:** forecast recurring patterns and prepare capacity.

Choose a metric tied to demand per unit of capacity. Average CPU can be useful, but request count per target, queue depth per worker, or another workload metric may describe demand better.

Scale-out should be responsive; scale-in should be cautious to prevent oscillation and interrupted work. Account for instance warm-up and application initialization.

## Stateless application pattern

For easy horizontal scaling:

- do not store user session state only on one instance;
- place durable objects in S3 and relational state in a database;
- use an external session/cache system if needed;
- make bootstrapping automatic and repeatable;
- make shutdown/draining safe;
- use idempotent background processing where possible.

## Deployment considerations

- Bake or version artifacts; do not depend on unpredictable latest packages.
- Use instance refresh, blue/green, or controlled rolling deployment patterns.
- Validate health before shifting all traffic.
- Keep rollback artifacts and procedures.
- Monitor error rate, latency, saturation, and business outcomes during change.

## Official references

- [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html)
- [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
- [Target tracking scaling policies](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html)
