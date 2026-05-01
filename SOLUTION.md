# Week 2 Deployment – Architecture Documentation

## Overview

This repository documents the cloud architecture deployed during Week 2 of the Cloud Engineering Bootcamp. The setup is a multi-availability-zone two-tier stack (web + application) hosted on AWS EC2, distributed via an Application Load Balancer.

## Repository Contents

| File | Description |
|------|-------------|
| `SOLUTION.md` | This file – project overview |
| `ARCHITECTURE.md` | Detailed component and network documentation |
| `architecture-diagram.png` | Visual architecture diagram |
| `SECURITY.md` | Security group rules and justifications |
| `COSTS.md` | Cost breakdown and optimization opportunities |
| `IMPROVEMENTS.md` | Proposed improvements |

## Summary

- **Cloud Provider:** AWS
- **Availability Zones:** 2
- **Compute:** 2x EC2 t3.small (Web Servers) + 2x EC2 t3.medium (App Servers)
- **Traffic Distribution:** Application Load Balancer
- **Security:** 4 Security Groups
- **Estimated Monthly Cost:** ~$106
