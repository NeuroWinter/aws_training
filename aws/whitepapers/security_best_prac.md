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


# Managing AWS Accounts, IAM users Groups and Roles.

- AWS Account.
    * This is the account that you create when you first sign up to AWS and this has root 
permissions to all AWS services.

    * There are different account strategies to maximise your security:
        1. Single Account.
            * Minimal overhead.
        2. Prod, Dev, Test Accounts.
            * Create 3 AWS accounts.
        3. Create an Account for each part of your business.
            * This can be per application or by department.
        4. Central Security with accounts for each part of business.
            * Here we create a central AWS account that controls security, things like DNS and
            AD. Then create accounts per application or department.
- IAM users.
    * IAM users are what you will use for your day to day interactions with AWS, each one has
    its own security credentials and are all controlled under a single AWS account. It is best
    practice to create an IAM user for each person or service that needs access to AWS. Then
    create tight controls in the AWS Account and apply them to the groups or users.

- IAM Groups.
    * IAM Groups are just a collection of IAM users in the same AWS account. 
    * You can apply a set of IAM Policies to a group.
    * All IAM Users in an IAM Groups will inherit all policies applied to that group.
    * It is best practice to great groups to provide users with access to AWS services.

## Delegating IAM roles and temp security credentials.

There are a few scenarios where you might want users or services that do not have AWS access
to use AWS services.

1. Application running on EC2 needs to write to S3.
2. Cross AWS Account access - Dev user needs to start a deployment in prod
3. Identify federation - AD users need AWS rights too.

IAM Roles and Temporary credentials will solve these use cases. 

IAM Roles allows you to give an AWS user/service rights to access some AWS service. To do this
the item (user/service) needs to assume the role in code, this will provide the requester with
temporary access to the AWS service that the role provides. These will have a exp time and will
automatically rotate. The benefits of this that you will not need to manage long term access
to AWS services.

## IAM Roles for EC2

Basically an admin can create an EC2 role for an application to access particular AWS resources.
This means that the application running on an EC2 instance has access, and the Developer does
not need access themselves. This also lets the admin give very fine grained access to only
the resources that the application needs.

PIC HERE

## Cross-Account Access

The main idea to this is that you can allow another account to access resources from one account
from another account. 

## Identity Federation

### Managing OS-level Access to Amazon EC2 Instances

In the Shared Responsibility model you own the OS credentials, however AWS provides ways to help
you set it up in the 1st place.

When you 1st create an EC2 instance from a stander AMI you are given the ssh or the RDP
credentials. Once you have initially authenticated you are then able to change the password to
whatever you like. 

AWS also provides asymmetric key pairs, these are not related to an AWS account or and IAM User,
and can be used on multiple EC2 instances. You can also opt to generate you own key pairs and 
upload only the public key to AWS. 

If you decided to let AWS generate your key pairs when you create the pairs initially you will be
prompted to download the public and the private key. AWS will not store the private key and if
you loose it you must generate a new pair.

Also remember that if you need you can disable the AWS EC2 pairs and implement your own auth.


# Secure Your Data

## Resource Access Authorization

There are two types of resource access:
- Resource policies.
    - These are where a particular user controls the resource and can give access to other users
    - The root AWS account always has complete access to all resources created under its account.
- Capability policies (user-based permissions).
    - This is where an IAM user has access directly or indirectly to a resource via IAM polices, either on the user itself or on a group a user is part of.
    - These can override resource policies.

## Storing and Managing Encryption Keys in the Cloud

WOW There is so much here I need to revisit .....

## Protecting Data at Rest

There are several threats that we need to think about here:

1. Accidental information disclosure.
1. Data integrity compromise.
1. Accidental deletion.
1. System, infra, hardware of software availability.

Here are AWS's solutions to those problems:

1. Accidental information disclosure.
    - Make data confidential and limit the number of users that have access to it 
    - Use encryption on EBS and RDS.
1. Data integrity compromise.
    - Limit the number of users that can access and modify the data, LEAST PRIVILEGED!!!
    - Perform data integrity checks and restore if there is a compromise.
1. Accidental deletion.
    - LEAST PRIVILEGED!
    - Enable MFA on delete.
    - Data integrity checks from above.
1. System, infra, hardware of software availability. 
    - Ensure that there is multiple AZs for your applications and their data.

Now lets look at some of the services AWS provides and the options they provide for protecting your data!

### S3
- Permissions:
    - There is bucket level and object level permissions along with IAM policies for creating, modifying and deleting data.
- Versioning:
    - While it is supported, it is disabled by default.
- Replication:
    - Every object in S3 is replicated across all AZs within the region.
    - How ever when something is deleted it will replicate across all AZs.
    - You can opt of reduced redundancy, for a lower cost.
- Backup:
    - This is not provided by AWS but you can implement this yourself.
- Encryption - Server side:
    - Supported but needs to be explicitly enabled.
    - Each object is encrypted with its own key and is encrypted with AES-256 the key itself is then encrypted with an AES-256 master key - This is all transparent to the user.
    - AWS manages the keys and they are rotated on a regular basis.
- Encryption - Client side:
    - You encrypt the data with whatever key and algo you want, AWS doesn't care and will have no idea if anything is encrypted or not.
### EBS

### RDS

### S3 Glacier

### DynamoDB

### EMR


