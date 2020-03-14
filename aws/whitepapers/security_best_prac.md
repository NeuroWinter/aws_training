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

## The different Layers of the security model.

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

In this model you own the access rights to your EC2 instances, but AWS helps you create these
access controls. They will provide you with your initial passwords and access to an EC2
instance or AMI image, and it is up to you to change these passwords.

Authention to EC2 instances are provided by AWS, using asymmetric key pairs, a.k.a Amazon
EC2 key pairs. Each user can have multiple EC2 keypairs, however these are not tied to the IAM
users or AWS at all. An EC2 key pair will only provide access to that single EC2 instance, and
no other AWS services. 
You can use your own generator like openSSH to generate these key pairs, or you can use AWS.
If you use AWS, they will not store the private key for you, and it is up to you to securely
store it.

When you create an EC2 linux instance using the cloud-init service the public key will be added to
the servers ~/.ssh/authorized_keys file, allowing you to ssh into the instance using the
private key.

When you create an EC2 Windows instance, then the admin password is encrypted with the public
key, a user can then request the password using the aws cli or management console, providing
the private key for decryption. 

All of this can be disabled of course.

### Container Services
These normally run on EC2 but you dont always manage the EC2.
You are responsible for setting up and managing network level access and access management. 
- RDS
- EMR
- Elastic Beanstalk

An Example of the shared responsibility model here is RDS, AWS looks after the backup and
recovery but you need to make sure your DR is up to scratch.

In this section you are responsible for data and firewall access too.

### Abstracted Services
AWS manages the platform and the underlying platform and exposes services via an API.
Meaning that the underlying infrastructure is shared but it is a multi tenanted system, with
secure data isolation.
- S3
- Glacier
- DynamoDB
- SQS
- SES

Abstracted assets are closly tied to IAM, meaning that it is up to you to make sure all your
assets are taged correctly and you have the correct access groups set up. Theses access groups
can be either at the user or the service level.

For some of the abstracted services you are also responsiable for ensuring encryption at rest
and in transit, like S3, where you need to enable HTTPS and encrypt the buckets themselves.

## Trust Validator tool.

AWS provides a tool that will check a few security best practices like checking open ports on 
EC2 instances, and that IAM and MFA are enabled.

## Designing an ISMS

Remember that an ISMS is an Information Security Management System, the main goal of this is to
protect what matters most to you, and your company. To create an ISMS we need to identify what
has the most value to you.

There are two types of assets: 
1. Essential elements:
  * Business information.
  * Process.
  * Activities.
2. Support:
  * Hardware.
  * Software.
  * Personal.
  * Partner Organizations.

Once you have categorised your assets into classes, work out the $ value of each of those
and what AWS services they depend on. This will better inform your decisions on creating a
standard for monitoring, and operating your ISMS on AWS.

Since Businesses change all the time it is a good idea to review this regally so that the
ISMS keeps in-line with the business.

You can break up the creation of an ISMS into several steps:
1. Define the scope
2. Define an ISMS policy
3. Select a risk assessment methodology.
4. Identify risks
5. Analyse risks
6. Address risks
7. Choose a Sec framework
8. Get management approval
9. Statement of applicability

