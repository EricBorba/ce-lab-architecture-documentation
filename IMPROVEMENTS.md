# Proposed Improvements

## 1. Add Auto Scaling

**Problem:** The current setup runs a fixed number of EC2 instances at all times, regardless of actual traffic. This wastes money during off-peak hours and could be insufficient during a traffic spike.

**Improvement:** Set up Auto Scaling Groups (ASGs) for both the web server and app server tiers, attached to the load balancer. Define a minimum of 1 instance per AZ and a maximum of 4, scaling based on CPU utilization or request count.

**Benefit:** Lower costs during low-traffic periods and automatic capacity during high-traffic events, without manual intervention.

---

## 2. Enforce HTTPS and Redirect HTTP

**Problem:** The load balancer currently accepts both HTTP (port 80) and HTTPS (port 443). Traffic sent over plain HTTP is unencrypted.

**Improvement:** Configure the ALB to automatically redirect all HTTP requests to HTTPS.

**Benefit:** All traffic between clients and the load balancer is encrypted, protecting users from interception. This is also a standard requirement in most real-world and production environments.

---

## 3. Add a Private Database Layer

**Problem:** The architecture only has web and application tiers. There is no dedicated database layer, meaning the app has nowhere to persist data in a structured, secure way.

**Improvement:** Add an Amazon RDS instance (e.g., PostgreSQL) in a private subnet with no public internet access. A dedicated security group would allow only the app servers to connect on port 5432.

**Benefit:** Persistent, managed data storage with proper network isolation. The database is never reachable from the internet, significantly reducing the attack surface.
