---
title: Load Balancing & Auto Scaling
draft: false
tags:
  - AWS
  - CLF-C02
  - Cloud
  - CloudPractitioner
  - EC2
  - ALB
  - NLB
  - AutoScaling
---
# What is an Elastic Load Balancer?
A service that helps manage and control the flow of inbound requests destined to a group of targets by distributing them evenly across a targeted resource group (we often think about EC2 instances but these can also be Lambda functions, IP addresses or containers). 

The targets can be situated across different Availability Zones or placed within a single one.

AWS ELB will act as the point for receiving incoming traffic from users and evenly distribute it across a greater number of instances. 

ELB is not a single point of failure because it is **highly available** and is comprised of multiple instances. It is **elastic**, since it is managed by AWS (it grows as you need it to).

## Load Balancer Types 
### Application Load Balancer
Flexible feature set for your web apps running HTTP and HTTPS protocols. Operates at the request level.
### Network Load Balancer
Ultra high performance while maintaining very low latency. Operates at the connection level, routing traffic to targets within your PVC. Listeners supported by NLB include TCP, UDP and TLS. 
### Classic Load Balancer
Used for apps built in the existing EC2 classic environment. Operates at both levels. Supports TCP, SSL/TLS, HTTP and HTTPS protocols.
## ELB Components
### Listeners
For every load balancer you must configure at least one listener. The listener defines how you inbound connections are routed to your target groups based on ports and protocols set as **conditions**.
### Target Groups
 A target group is a group of your resources that you want your ELB to route requests to.
### Rules
Associated to each listener that you have configured within your ELB.

Overall. Each ELB contains multiple listeners > each contains multiple rules > each rule can have multiple conditions > all conditions of the rule equate an action 
### Health Checks
Performed against the resources defined within the target group. The ELB contacts and if it does not receive an answer it stops sending traffic there.
### Internet-Facing ELB
They have a public DNS name, in addition to an internal IP address.  
### Internal ELB
Only has internal IP address, so it only serves requests originating from within the VPC itself.
# SSL Server Certificates
Since application load balancers support HTTP and HTTPS the HTTPS setting requires a little more setup.

The encrypted channel is set between clients initiating the request and your ALB. The ALB needs a server certificate and an associated security policy. SSL (Secure Sockets Layer) is a cryptographic protocol, much like TLS.

The server certificate used by an ALB is an X.509 certificate, a digital ID provisioned by a Certificate Authority as the AWS Certificate Manager (ACM).

There are 4 options when selecting HTTPs as your listener:
- Choose a certificate from ACM 
- Upload a certificate to ACM
- Choose a certificate from IAM 
- Upload a certificate to IAM

The IAM options are usually used when deploying the ELBs in regions that are not supported by ACM.

# Auto Scaling in AWS
First, there is EC2 Auto Scaling and AWS Auto Scaling (focusing on services like ECS, DynamoDB and Amazon Aurora).

EC2 auto scaling allows you to scale out and scale in the size of your EC2 fleet automatically.

Advantages:
- Automatically provision resources when necessary (no need for someone to remove and add them manually).
- Greater customer satisfaction due to service availability
- Cost reduction, due to being able to reduce the amount of resources when the demand drops
# Auto Scaling Components
The launch configuration defines how an Auto Scaling Group builds new EC2 instances
- AMI
- Instance type
- Spot instances or not
- If and when public IP addresses should be used
- If any user data is on first boot
- What storage volume config should be used
- What security groups should be used
A **launch template** is a more sophisticated and new type of launch configuration that allows you simplify the launch process for instances in scaling groups

The **auto scaling group** defines:
- The desired capacity and other limitations of the group using scaling policies
- Where the group should scale resources, such as an AZ

# Using ELB and Auto Scaling Groups together
Overall, these two go hand in hand to provide optimal efficiency from both a performance and cost perspective. One without the other, however, can cause operational burden (auto scaling without ELB requires you to manually distribute load, ELB withou auto scaling required you to manually add or remove instances to redirect incoming traffic to).