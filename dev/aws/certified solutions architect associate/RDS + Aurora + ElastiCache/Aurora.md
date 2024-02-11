- Aurora is a proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database)
- Aurora is “AWS cloud optimized” and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10GB, up to 128 TB.
- Aurora can have up to 15 replicas and the replication process is faster than MySQL (sub 10 ms replica lag)
- Failover in Aurora is instantaneous. It’s HA (High Availability) native.
- Aurora costs more than RDS (20% more) – but is more efficient

## Aurora High Availability and Read Scaling

- 6 copies of your data across 3 AZ:
	- 4 copies out of 6 needed for writes  
	- 3 copies out of 6 need for reads  
	- Self healing with peer-to-peer replication • Storage is striped across 100s of volumes
- One Aurora Instance takes writes (master)
- Automated failover for master in less than 30 seconds
- Master + up to 15 Aurora Read Replicas serve reads
- Support for Cross Region Replication

![[Pasted image 20240211134408.png]]

## Aurora DB Cluster

![[Pasted image 20240211134544.png]]
Master is the only thing that writes to the storage, and because the master can change and failover, Aurora providers `Writer Endpoint`(which is a DNS name). It always points at the Master. The client still talks to the writer endpoint even if the master fails-over and will automatically direct to the right instance.
You can have a lot of Read Replicas in Aurora, and it can have Auto Scaling on top of those Read Replicas. Because of auto scaling, it can be hard for the application to keep track of where the replicas are, what are the urls or how to connect to them.
**The solution is `Reader Endpoint`. It helps with connection load balancing and it connects automatically to all the read replicas**. So, anytime the client connects to the Read Endpoint, it will get connected to one of the Read Replicas and there will be load balancing done this way. Just keep in mind that the load balancing happens at the connection level, not the statement level.

## Features of Aurora

- Automatic fail-over  
- Backup and Recovery
- Isolation and security
- Industry compliance
- Push-button scaling
- Automated Patching with Zero Downtime  
- Advanced Monitoring  
- Routine Maintenance  
- Backtrack: restore data at any point of time without using backups

## Aurora Auto Scaling

![[Pasted image 20240211140333.png]]
If more read requests come in, Aurora DB have an increased CPU Usage. In this case, we can setup Replica Auto Scaling. This will add Aurora Replicas and the Reader Endpoint will be extended to cover the auto scaled Replicas.

## Aurora Custom Endpoints

![[Pasted image 20240211140614.png]]
Notice that some Aurora Read Replicas are bigger than others. We would create a `Custom Endpoint`.
- Define a subset of Aurora Instances as a Custom Endpoint
- Example: Run analytical queries on specific replicas
- The `Reader Endpoint` is generally **NOT** used after defining `Custom Endpoints`. So you would need to setup many custom endpoints for many different kinds of workloads.

## Aurora Serverless

- Automated database instantiation and auto-scaling based on actual usage
- Good for infrequent, intermittent or unpredictable workloads
- No capacity planning needed
- Pay per second, can be more cost-effective

![[Pasted image 20240211141136.png]]

## Aurora Multi-Master

- In case you want **continuous write availability** for the writer nodes
- Every node does Read / Write, instead of promoting a Read Replica as the new master

## Global Aurora

- **Aurora Cross Region Read Replicas**:
	- Useful for disaster recovery  
	- Simple to put in place
- **Aurora Global Database (recommended)**:
	- 1 Primary Region (read / write)
	- Up to 5 secondary (read-only) regions, replication lag is less than 1 second
	- Up to 16 Read Replicas per secondary region
	- Helps for decreasing latency
	- Promoting another region (for disaster recovery) has an RTO of < 1 minute
	- **Typical cross-region replication takes less than 1 second**

![[Pasted image 20240211141522.png]]

## Aurora Machine Learning

- Enables you to add ML-based predictions to your applications via SQL
- Simple, optimized, and secure integration between Aurora and AWS ML services
- Supported services  
	- Amazon SageMaker (use with any ML model)
	- Amazon Comprehend (for sentiment analysis)
- You don’t need to have ML experience
- Use cases: fraud detection, ads targeting, sentiment analysis, product recommendations

![[Pasted image 20240211141631.png]]