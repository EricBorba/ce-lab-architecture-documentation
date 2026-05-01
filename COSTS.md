# Cost Analysis

## Current Monthly Costs (On-Demand Pricing)

| Component | Quantity | Unit Cost | Monthly Total |
|-----------|----------|-----------|---------------|
| EC2 t3.small – Web Servers | 2 | $15/mo | $30 |
| EC2 t3.medium – App Servers | 2 | $30/mo | $60 |
| Application Load Balancer | 1 | ~$16/mo | ~$16 |
| Data Transfer (estimate) | – | ~$0.09/GB | ~$5 |

**Estimated Total: ~$111/month**

## Optimization Opportunities

### 1. Reserved Instances
Committing to a 1-year Reserved Instance saves approximately 38–40% on EC2 costs.

| Instance Type | On-Demand | 1-Year Reserved | Monthly Savings |
|---------------|-----------|-----------------|-----------------|
| t3.small x2 | $30 | ~$18 | ~$12 |
| t3.medium x2 | $60 | ~$37 | ~$23 |
| **Total** | **$90** | **~$55** | **~$35/mo** |

### 2. Auto Scaling
Replace the fixed 4-instance setup with Auto Scaling Groups. Set a minimum of 1 instance per type per AZ during off-peak hours, scaling up as needed. This could reduce EC2 costs by up to 50% during low-traffic periods.

### 3. Use t3.small for App Servers in Dev/Test
If this environment is used for development or testing (not production), downsizing app servers from t3.medium to t3.small saves $30/month with minimal performance impact for low traffic.

## Summary

| Scenario | Est. Monthly Cost |
|----------|------------------|
| Current (on-demand) | ~$111 |
| With Reserved Instances | ~$76 |
| With Auto Scaling (avg utilization 60%) | ~$80 |
