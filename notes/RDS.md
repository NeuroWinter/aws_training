### Overview

- This is an AWS managed relational Database Service.
- The following DBs are supported:
    - Postgres
    - MySQL
    - MariaDB
    - Oracle
    - Microsoft SQL
    - Aurora
- RDS benefits:
    - Automated provisioning and patching
    - Automatic backups
    - Point-in-time Restore
    - Monitoring Dashboards
    - Read-Replicas
    - Multi AZ support
    - Maintenance windows
    - Vertical and Horizontal scaling
    - Backed by GP2 and IO1 drives
    - You can use IAM authentication

NOTE: When you are using RDS you will not be able to SSH into the server that is running the DB

### RDS Backups
- Backups are enabled by default in RDS
- Automated backups:
    - Daily during the defined maintenance window
    - Transaction logs every 5 min, this means that you can restore to almost any point in time
    - Default of 7 day retention
    - These will all be deleted if you delete your RDS instance.
- You can also manually backup using a snapshot. These manual snapshots are retained until you manually delete them.


### Read Replicas

These can be in the same AZ, Cross AZ or cross region

Read replicas are async this means that they will be eventually consistent, meaning you might get old data when reading from them.

If you use Read Replicas your application must have them in their connection string.

The use case for Read Replicas is a reporting application where the application is not writing to the db and just reading.

Read Replicas can **only** use the SELECT SQL command.

Also note that the can be a network cost when using read Replicas, but that is only when your replica is in another AZ than your writer.

#### RDS DR Multi AZ
- This uses a sync replication from the master RDS in one AZ to a slave in another AZ.
- We will have a single DNS name for the DB, and it will automatically fail over to the slave when the master fails

### RDS Encryption
- At rest encryption:
    - You can encrypt both the master and read replicas
    - If the master is not encrypted then all the read replicas will not be encrypted as well.
    - Encryption is defined at Launch time.
    - You can only enable transparent data encryption for Oracle and SQL server.
- In Flight Encryption:
    - SSL certs can be used
    - You need to provide the SSL certs when connecting to the DB
    - To **enforce** it:
        - In Postgres set `rds.force_ssl=1`
        - In MySQL you need to the a command on the DB.

### RDS Security
- Encryption at rest:
    - This can only be done when you create the DB or when you snapshot an encrypted DB and restore it to an encrypted instance.

Your Responsibilities are as follows:
- Ports/IP/Security groups for inbound connections
- In DB users either using IAM or DB users
- Making sure your DB is not public
- The parameter groups to ensure ssl in enforced

AWS manages the following:
- Making sure there is no SSH access
- DB patching
- OS patching
- Making sure there is no access to the underlying instance
