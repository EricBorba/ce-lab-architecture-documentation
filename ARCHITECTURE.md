# Architecture Documentation

## Diagram

![Architecture Diagram](/screenshots/architecture-diagram.png)

## Description

The deployment spans two AWS Availability Zones for fault tolerance. Public internet traffic enters through an Application Load Balancer, which distributes HTTP/HTTPS requests to a **Web Server** in each AZ. The web server handles HTTP serving and forwards application logic to the **Application Server** (Node.js on port 3000) within the same AZ.

## Network Flow

```
Internet
    │
    ▼ (HTTP/HTTPS)
Application Load Balancer
    │                        │
    ▼ HTTP/HTTPS             ▼ HTTP/HTTPS
  AZ-1                     AZ-2
  ├── Web Server (t3.small)     ├── Web Server (t3.small)
  └── App Server (t3.medium)    └── App Server (t3.medium)
        Node.js :3000                  Node.js :3000
```

## Component Inventory

| Component | Type | Size | Purpose | Monthly Cost (est.) |
|-----------|------|------|---------|---------------------|
| Web Server – AZ1 | EC2 | t3.small | Serve HTTP | $15 |
| Web Server – AZ2 | EC2 | t3.small | Serve HTTP | $15 |
| App Server – AZ1 | EC2 | t3.medium | Run Node.js | $30 |
| App Server – AZ2 | EC2 | t3.medium | Run Node.js | $30 |
| Application Load Balancer | ALB | N/A | Distribute traffic across AZs | ~$16 |
| Security Groups | 4 SGs | N/A | Network security | Free |

**Total estimated monthly cost: ~$106**

## Availability Zones

| Zone | Web Server | App Server |
|------|------------|------------|
| AZ-1 | 1x t3.small | 1x t3.medium |
| AZ-2 | 1x t3.small | 1x t3.medium |

Having the full stack replicated across two AZs ensures the application stays available if one zone experiences an outage.
