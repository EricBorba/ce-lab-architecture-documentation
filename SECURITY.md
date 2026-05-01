# Security Documentation

## Security Groups Overview

The architecture uses 4 security groups, one per layer per role.

| Security Group | Attached To | Purpose |
|----------------|-------------|---------|
| sg-loadbalancer | Application Load Balancer | Allow public HTTP/HTTPS traffic |
| sg-webserver | Web Server EC2s (both AZs) | Allow traffic only from load balancer |
| sg-appserver | App Server EC2s (both AZs) | Allow traffic only from web servers |
| sg-internal | Internal communication | Allow inter-tier traffic as needed |

## Security Group Rules

### Load Balancer SG (sg-loadbalancer)

| Direction | Protocol | Port | Source | Justification |
|-----------|----------|------|--------|---------------|
| Inbound | TCP | 80 | 0.0.0.0/0 | Allow HTTP from internet |
| Inbound | TCP | 443 | 0.0.0.0/0 | Allow HTTPS from internet |
| Outbound | TCP | 80/443 | sg-webserver | Forward requests to web servers |

### Web Server SG (sg-webserver)

| Direction | Protocol | Port | Source | Justification |
|-----------|----------|------|--------|---------------|
| Inbound | TCP | 80 | sg-loadbalancer | Accept traffic only from load balancer |
| Inbound | TCP | 443 | sg-loadbalancer | Accept HTTPS only from load balancer |
| Outbound | TCP | 3000 | sg-appserver | Forward requests to app servers |

### App Server SG (sg-appserver)

| Direction | Protocol | Port | Source | Justification |
|-----------|----------|------|--------|---------------|
| Inbound | TCP | 3000 | sg-webserver | Accept traffic only from web servers |
| Outbound | All | All | 0.0.0.0/0 | Allow outbound for updates and dependencies |

## Key Security Decisions

**No instance is directly reachable from the internet.** Web servers only accept traffic from the load balancer, and app servers only accept traffic from web servers. This creates a proper layered defense.

**Principle of least privilege** is applied throughout: each security group allows only the minimum ports and sources required for that tier to function.

**No SSH (port 22) inbound rule** is defined. Instance access for maintenance should be handled via AWS Systems Manager Session Manager to avoid exposing SSH publicly.
