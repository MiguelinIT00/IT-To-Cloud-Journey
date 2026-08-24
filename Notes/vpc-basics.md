# Amazon VPC Deep Dive

## Mental model

A VPC is a logically isolated network in an AWS Region. A subnet is an Availability Zone-scoped slice of its IP address range. Routing determines the next hop. Security groups and network ACLs filter traffic. DNS and endpoints determine how workloads find and privately reach services.

## CIDR planning

Example learning layout for `10.20.0.0/16`:

| AZ | Public | Private application | Isolated database |
|---|---|---|---|
| A | `10.20.0.0/24` | `10.20.10.0/24` | `10.20.20.0/24` |
| B | `10.20.1.0/24` | `10.20.11.0/24` | `10.20.21.0/24` |

Leave room for growth and avoid overlapping ranges if VPCs, on-premises networks, or partner networks may later connect.

## What makes a subnet public or private

- A **public subnet** has a route to an internet gateway. An IPv4 resource still needs a public IPv4/Elastic IP, or a public-facing intermediary, to communicate directly through it.
- A **private subnet** has no route directly to an internet gateway.
- A private IPv4 workload can initiate internet traffic through a NAT gateway placed in a public subnet.
- An **isolated subnet** has no general internet route; it can still use explicitly configured private routes and endpoints.

Naming a subnet `public` does not change its routing.

## Route evaluation

Each route has a destination and target. AWS chooses the most specific matching route. Common targets include:

- `local` for traffic inside the VPC CIDR;
- internet gateway for direct internet routing;
- NAT gateway for private IPv4 egress;
- gateway or interface VPC endpoint for private service access;
- VPC peering or Transit Gateway for connected networks;
- virtual private gateway for some VPN designs.

## Internet gateway versus NAT gateway

| Internet gateway | NAT gateway |
|---|---|
| Attached to the VPC | Created in a subnet |
| Target for public subnet routes | Target for private subnet IPv4 egress routes |
| Supports direct inbound/outbound for publicly addressed resources when controls allow | Supports connections initiated by private resources; not unsolicited inbound access |
| No hourly gateway charge, though transfer charges can apply | Hourly and data-processing charges apply |

For resilient zonal design, consider one NAT gateway per AZ and route each private subnet to the NAT gateway in the same AZ. This costs more than one shared NAT gateway. VPC endpoints may reduce exposure and NAT processing for supported services.

## Security groups and NACLs

Security groups are stateful and apply to network interfaces. Network ACLs are stateless, apply to subnets, have numbered allow/deny rules, and require return traffic to be allowed explicitly.

Example tiered security groups:

- ALB SG: inbound TCP 443 from intended clients; outbound app port to application SG.
- Application SG: inbound app port from ALB SG; outbound database port to database SG and required service destinations.
- Database SG: inbound database port from application SG only.

Avoid using broad CIDRs where a security-group reference expresses the trust relationship more precisely.

## DNS

VPC DNS settings affect whether resources receive and resolve AWS-provided hostnames. Route 53 Resolver handles DNS for VPC workloads and can connect to on-premises DNS using inbound/outbound Resolver endpoints.

## Private access to AWS services

- **Gateway endpoints** support services such as S3 and DynamoDB and are used through route tables.
- **Interface endpoints** create elastic network interfaces powered by AWS PrivateLink and are protected with security groups.

Endpoint policies and service resource policies can further limit access. An endpoint is not a replacement for IAM authorization.

## Packet troubleshooting checklist

1. Identify source IP/SG, destination IP/SG, protocol, and port.
2. Confirm name resolution returns the intended destination.
3. Confirm source and destination routes, including a return path.
4. Check security groups on each relevant network interface.
5. Check NACL inbound and outbound rules, including ephemeral ports.
6. Confirm the process is listening and the host firewall allows traffic.
7. Use VPC Flow Logs and service logs to test the hypothesis.
8. Check Transit Gateway, peering, VPN, endpoint policy, or DNS when relevant.

## Official references

- [VPC route tables](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html)
- [Internet gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
- [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html)
