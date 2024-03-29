- RDS stands for Relational Database Service
- It's a managed DB service for DB use SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS
	- Postgres
	- MySQL
	- MariaDB
	- Oracle
	- Microsoft SQL Server
	- Aurora (AWS Proprietary database)

## Advantage over using RDS vs deploying DB on EC2

- RDS is a managed service:
	- Automated provisioning, OS patching
	- Continuous backups and restore to specific timestamp (Point in Time Restore)!
	- Monitoring dashbaords
	- Read replicas for improved read performance
	- Multi AZ setup for DR (Disaster Recovery)
	- Maintenance windows for upgrades
	- Scaling capability (vertical and horizontal)
	- Storage backed by EBS (`gp2` or `io2`)
- BUT you can't SSH into your instances

## Storage Auto Scaling

- Helps you increase storage on your RDS DB instance
- When RDS detects you are running out of free database storage, it scales automatically
- You have to set **Maximum Storage Threshold** (maximum limit for DB storage)
- Automatically modify storage if:
	- Free storage is less than 10% of allocated storage
	- Low-storage lasts at least 5 minutes
	- 6 hours have passed since last modification
- Useful for applications with **unpredictable workloads**
- Supports all RDS database engines (MariaDB, MySQL, PostgresSQL, SQL Server, Oracle)

![[Pasted image 20240211102039.png]]

## RDS Read Replicas for read scalability

### Read Replicas

- Up to 15 Read Replicas
- Within AZ, Cross AZ or Cross Region
- Replication is **ASYNC**, so reads are eventually consistent
- Replicas can be promoted to their own DB
- To use Read Replicas, applications must update the connection string to leverage read replicas

![[Pasted image 20240211102432.png]]

### Use Cases
- You have a production database that is taking on normal load
- You want to run a reporting application to run some analytics
- You create a Read Replica to run the new workload there
- The production application is unaffected
- Read replicas are used for SELECT only kind of statements (not INSERT, UPDATE, DELETE)

![[Pasted image 20240211102811.png]]

## Network Cost

- In AWS, there's a network cost when data goes from one AZ to another
- **For RDS Read Replicas within the same region, you don't pay that fee**

![[Pasted image 20240211103015.png]]

## RDS Multi AZ (Disaster Recovery)

- **SYNC** replication
- One DNS name - automatic app failover to standby
- Increase **availability**
- Failover in case of loss of AZ, loss of network, instance or storage failure
- No manual intervention in apps
- Not used for scaling (just for standby, no one can read or write to it)
- Note: The Read Replicas can be setup as Multi AZ for Disaster Recovery (DR)

![[Pasted image 20240211103124.png]]

## From Single-AZ to Multi-AZ

- Zero downtime operation (no need to stop the DB)
- Just click on "modify" for the database and enable multi-AZ
- The following happens internally:
	- A snapshot is taken
	- A new DB is restored from the snapshot in a new AZ
	- Synchronization is established between the two databases

![[Pasted image 20240211103527.png]]

## RDS Custom

- **Managed Oracle and Microsoft SQL Server Database with OS and database customization**
- RDS: Automates setup, operation, and scaling of database in AWS
- Custom: access to the underlying database and OS so you can
	- Configure settings
	- Install patches  
	- Enable native features  
	- Access the underlying EC2 Instance using **SSH** or SSM **Session Manager**
- **De-activate Automation Mode** to perform your customization, better to take a DB snapshot before
- RDS vs. RDS Custom  
	- RDS: entire database and the OS to be managed by AWS
	- RDS Custom: full admin access to the underlying OS and the database