# AWS Load Balancers

## Overview
A Load Balancer distributes incoming traffic across multiple servers to improve availability and reliability.
---

## Types of Load Balancers

### 1. Application Load Balancer (ALB)

- Works at Layer 7 (HTTP/HTTPS)
- Supports path-based routing
- Best for web applications

### 2. Network Load Balancer (NLB)
- Works at Layer 4 (TCP/UDP)
- High performance and low latency
- Best for real-time applications

### 3. Classic Load Balancer (CLB)
- Legacy
- Limited features compared to ALB/NLB
---

## Key Concepts

### Target Groups
- Group of EC2 instances
- Load balancer sends traffic here

### Health Checks
- Ensures instances are healthy
- Unhealthy instances are removed from rotation

---

## Example Scenario
User → Load Balancer → Multiple EC2 Instances
If one instance fails:
- Traffic is redirected to healthy instances automatically
---

## Why Use It?
- High availability
- Fault tolerance
- Scalability
 
