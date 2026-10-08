# Aws-Vpc-Architecture
# Multi-Tier AWS VPC Architecture

A secure 3-tier web application on AWS using two peered VPCs.

## Architecture Flow
Current: Internet → ALB → Public EC2 (Web tier) → VPC Peering → Private EC2 (App tier) → RDS (Database tier)

Planned: User → Route 53 (DNS) → CloudFront (CDN) → ALB → Public EC2 → Private EC2 → RDS

## Public VPC
- 2 public subnets in 2 different Availability Zones
- 2 EC2 instances running the web application
- Application Load Balancer with a Target Group
- Internet Gateway and custom Route Tables
- NACLs (subnet level) and Security Groups (instance level)

## Private VPC
- 4 subnets: 1 for EC2 (App tier), 2 for RDS, 1 public subnet for the NAT Gateway
- NAT Gateway gives the private EC2 internet access for updates
- RDS is private and never exposed to the internet

## Security
- RDS accepts traffic only from the private EC2 security group
- Private resources have no public IP
- Security Groups and NACLs follow least privilege

## AWS Services Used
VPC, Subnets, Route Tables, Internet Gateway, NAT Gateway, VPC Peering, EC2, ALB, RDS, NACL, Security Groups

## Screenshots

### VPCs
![VPCs](screenshots/vpcs.png)

### VPC Peering (Active)
![Peering](screenshots/peering.png)

### EC2 Instances
![EC2](screenshots/ec2-instances.png)

### Load Balancer
![ALB](screenshots/alb.png)

### Target Group (2 Healthy Targets)
![Target Group](screenshots/target-group.png)

### RDS Database
![RDS](screenshots/rds.png)

### Private EC2 connected to RDS
![Private EC2 to RDS](screenshots/private-ec2-to-rds.png)

## Planned Improvement: CDN (Amazon CloudFront)
CloudFront is not configured yet because of free tier account limits.
- Create a CloudFront distribution with the ALB as the origin
- Users get content from the nearest edge location, so the site loads faster
- Static files are cached, so the servers get less traffic
- Restrict the ALB security group to CloudFront only
- Optional: add AWS WAF to block bad traffic

## Planned Improvement: DNS (Amazon Route 53)
Route 53 is not configured yet because of free tier account limits.
- Create a Hosted Zone for the domain
- Create an Alias record that points the domain to CloudFront or the ALB
- Enable Health Checks so traffic goes only to healthy resources
- Use routing policies (Weighted, Failover, Latency) for high availability
