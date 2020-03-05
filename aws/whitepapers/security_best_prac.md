# Shared Responsibility model
- AWS customers are responsible for their own security.
- AWS provides a secure infrustructure but you need to secure the data, Systems and platforms
- IAM lets you manage users and user permissions.
- For EC2 as an example you need to look after the following:
    - AMIs
    - OS
    - Applications
    - Data in transit
    - Data at rest
    - Data stores
    - Credentials
    - Policies and configuration
- AWS looks after the following:
    - Facilities
    - Physical security of hardware
    - Network infrastructure
    - Virtualization infrastructure

# AWS Global Secure Infrastructure

## IAM
- Used to create and manage users access to console and api.

### Regions, Availability Zones, and Endpoints
- Regions manage country regulations.
- When you store data in one region its not available in another.
- You need to replicate the data outside of the region.
- Regions are made up of Availability Zones. 
- Availability Zones are designed to be fault tolerant.
- You are responsible for choosing the AZs in the same region




### Infrastructure Services
These allow you to create infrastructure in the cloud similar to on prem
- EC2
- EBS
- Auto Scaling
- VPC

These all run on the AWS global infrastructure, and will have different availabilities. 
These will always run within the region where you launched them. 
You can create a highly resilient system by deploying these services across Availability Zones.

#### Customer Managed Security layers:
- Client-side data encryption and data integrity authentication
- Server-side encryption (File system and/or data)
- Network Traffic protection (encryption, integrity, identity)
- OS, network, firewall
- Platform and application management
- Customer Data



### Container Services
These normally run on EC2 but you dont always manage the EC2.
You are responsible for setting up and managing network level access and access management. 
- RDS
- EMR
- Elastic Beanstalk

### Abstracted Services
AWS manages the platform and the underlying platform and exposes services via an API.
Meaning that the underlying infrastructure is shared but it is a multi tenanted system, with
secure data isolation.
- S3
- Glacier
- DynamoDB
- SQS
- SES

