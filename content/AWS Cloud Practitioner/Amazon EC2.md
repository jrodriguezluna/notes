---
title: Amazon EC2
draft: false
tags:
  - AWS
  - EC2
  - Cloud
  - CloudPractitioner
  - CLF-C02
---
# Amazon EC2

Amazon Elastic Cloud Compute. AKA EC2. Allows you to deploy virtual servers within your AWS environment.

EC2 Components:
- Amazon Machine Images (AMI) 
- Instance types
- Instance purchasing options
- Tenancy
- User data
- Storage options
- Security
## AMI: Amazon Machine Images
These are templates of preconfigured EC2 instances, which allow for quick deployment. From a high lever perspective they are an **image baseline** that include an operating system and applications along with any custom config. 

We can get them in many ways:
1. AWS provides different AMIs by default, including Linux distros, Mac and Windows images. 
2. We can create our own AMIs! this way we can quickly deploy personalized new instances.
3. Aside from that... the AWS Marketplace is an online store which allows you to purchase AMIs from trusted vendors. 
4. Community AMIs are also a thing, anyone can share their AMIs with the rest.
## Instance Types
The size of the instance based on a number of parameters. For example: vcpus, architecture (f.e. x86_64, i386), memory (RAM), storage capacity, storage type (HDD, SSD) , network performance (rate of network transfer).

The instance type you choose really just depends on how appropriate it is for your needs. There are multiple types of instance types:
- General purpose: balanced mix of CPU, memory and storage. Ideal for small to medium databases. 
- Compute optimized: greater focused on compute power. Ideal for apps that require high performance processors (f.e. batch processing and machine learning).
- Memory optimized: used for large-scale enterprise class in-memory applications (in other words, used for applications that need lots of high speed RAM), such as real time processing of unstructured data.
- Accelerated computing: utilizes hardware accelerators or co-processors to perform floating-point calculations faster and more efficiently (in other words, these instance types use GPUs to optimize computing power).
- Storage optimized: uses SSD-backed instance storage for low-latency and very high I/O performance, IOPS (Input Output Operations Per Second). Useful for data file systems and log processing apps.
- HPC optimized: designed for high performance computing (HPC) workloads (these are different from accelerated computing in that HPC requires high end computing power combined with fast networking, GPUs, on the other hand, aid with processing specific types of math such as AI model training and cryptography)
## Instance Purchasing Options
 There are different ways to purchase EC2 instances to optimize their cost:
 - On-demand instances: 
	 - These can be launched at any time
	 - Can be used for as long as needed
	 - Flat rate based on the instance type, it is build by the second
	 - Typically used for short-term workloads (that cannot be interrupted)
	 - Good for testing and dev environment
 - Spot instances
	 - The AWS cloud is huge so there might be unused EC2 capacity. Spot instances leverage this capacity to offer discounts over on-demand pricing.
	 - Variable hourly price set by AWS based on supply and demand
	 - You can set a max price (per instance/hour) that you are willing to pay
	 - Eventually undergoes "Spot Instance Interruption", where the instance terminates/stops/hibernates (based on you configuration) either because spot capacity is no longer available or because it has exceeded your max price
	 - Only good for apps that are resilient to interruptions
 - Reserved instances
	 - Purchase a discounted on-demand instance for a set period
	 - Ideal for long-term and predictable workloads (since you do not w)
	 - Must be purchased in 1 to 3 year commitments
	 - Savings based on how much you wish to pay upfront (All upfront vs partial upfront vs no upfront
	 - Standard vs convertible reserved instances (both can be modified in size of same instance family or availability zone, but only convertible can change size, family and platform )
 - On-demand capacity reservations
	 - Reserve capacity for your EC2 instances based on attributes such as instance types or platform within an AZ
## Tenancy
Runs with **shared tenancy** by default. Which means that it runs in any available host with resources required for your instance type (other users may have instances running on the same host).
This is no problemo (other users on the same host cannot access your instances), although you might need a dedicated host for certain reasons (policies, security compliance, etc)

**Dedicated tenancy**: 
- Dedicated instances: hosted on hardware that no other customer can access (it is more expensive). In this case, you do not have control over the physical server itself, every time it is stopped the instance might be moved to another physical machine.
- Dedicated hosts: you have control over the hardware. It is useful for strict regulations or bring your own licence compliance.
## User Data
When creating an EC2 instance this option allows you to enter commands that will run during the first boot cycle of that instance (f.e. pulling down software, updating OS).
## Storage Options
- Persistent storage: available through EBS volumes. 
	- These are not physically attached ( they are network/logically attached) and are their own devices. 
	- Thus it can be freely detached
	- This storage is replicated within the same availability zone for resiliency.
	- Other options include: point in time snapshots through S3, EBS encryption
- Ephemeral storage: created by EC2 instances using local storage. 
	- These are physically attached and cannot be detached. 
	- If the instance hibernates, stops or is terminated, the data is lost (it can be rebooted). 
	- It is faster
## Security
Security groups: essentially, instance level firewalls (restricts ingress and egress traffic).
Key-pair: you can use it to freely connect to your instance

# EC2 Image Builder 
Automates the creation of virtual images for ec2 instances and containers. It does this by having a robust image builder pipeline (customize & install software > enforce security measures > test images > distribute to regions). 

The pipeline goes through:
- Setup (name, description, tags, scheduler)
- Recipe: Instructions to follow when creating an AMI
- Components: Described using a component document YAML (build, validate and test phases)
	- Build component
	- Test component

Image builder integrates with AWS Organizations, it allows accounts with permissions to launch EC2 instances from AMIs

# AWS Elastic Beanstalk
An AWS managed service that allows to upload source code for a web app and set configurations for its environment. Beanstalk takes charge from there, including things like deploying, provisioning of AWS resources (like EC2 instances, auto scaling groups, RDS database instances, load balancers, etc), monitoring and scaling.

Good for those that are not very familiar with AWS.

Support and maintain your app environment as you would with a custom-build environment outside of Beanstalk

Common maintenance tasks can be performed through the Beanstalk dashboard

Elastic Beanstalk is free but the deployed infrastructure isn't
## Components
- Environment: aame, URL and a description. Refers to the collection of resources created by Beanstalk
- Versions: a reference to a labeled code baseline (often a reference to an object stored in S3)
- Configurations: a collection of settings that dictate how your environment resources are provisioned and will behave. Saved configs can be used as templates for creating new environments in the future
## Environment tiers: 
- Web Server: Runs a website that processes HTTP requests (Route 53, Elastic Load Balancing, Auto Scaling, EC2, Security Groups)
- Worker: Used by applications that perform back-end processing tasks that interact with Amazon SQS (SQS Queue, IAM service role, Auto Scaling, EC2)

# AWS Lambda
Serverless compute service. Meaning, it allows you to run code without having to provision infrastructure for it. 

