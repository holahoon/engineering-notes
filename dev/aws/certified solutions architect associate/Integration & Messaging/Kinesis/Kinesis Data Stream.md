It is a way to stream big data in your systems. It is made of multiple shards.

![[Pasted image 20240223142948.png]]

- Retention between 1 day to 365 days
- Ability to reprocess (replay) data
- Once data is inserted in Kinesis, it can't be deleted (immutability)
- Data that shares the same partition goes to the same shard (ordering)
- Producers: AWS SDK, Kinesis Producer Library (KPL), Kinesis Agent
- Consumers:
	- Write your own: Kinesis Client Library (KCL), AWS SDK
	- Managed: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics,

### Capacity Modes
- **Provisioned mode:**
	- You choose the number of shards provisioned, scale manually or using API
	- Each shard gets 1 MB/s in (or 1000 records per second)
	- Each shard gets 2 MB/s out (classic or enhanced fan-out consumer)
	- You pay per shard provisioned per hour
- **On-demand mode:**
	- No need to provision or manage the capacity
	- Default capacity provisioned (4 MB/s in or 4000 records per second)
	- Scales automatically based on observed throughput peak during the last 30 days
	- Pay per stream per hour & data in/out per GB

## Security

- Control access / authorization using IAM policies
- Encryption in flight using HTTPS endpoints
- Encryption at rest using KMS
- You can implement encryption / decryption of data on client side (harder)
- VPC Endpoints available for Kinesis to access within VPC
- Monitor API calls using CloudTrail

![[Pasted image 20240223151037.png]]