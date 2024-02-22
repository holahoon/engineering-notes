SQS stands for Simple Queue Service.
## What's a queue?

![[Pasted image 20240222111124.png]]

## Standard Queue

 - Oldest offering (over 10 years old)
 - Fully managed service, used to **decouple applications**
 - Attributes:
	 - Unlimited throughput, unlimited number of messages in queue
	 - Default retention of messages: 4 days, maximum of 14 days
	 - Low latency (< 10 ms on publish and receive)
	- Limitation of 256KB per message sent
- Can have duplicate messages (at least once delivery, occasionally)
- Can have out of order messages (best effort ordering)

## Producing Messages

- Produced to SQS using the SDK (SendMessage API)
- The message is **persisted** in SQS until a consumer deletes it
- Message retention: default 4 days, up to 14 days

- Example: send an order to be processed
	- Order id
	- Customer id
	- Any attributes you want
- SQS standard: unlimited throughput

![[Pasted image 20240222112336.png]]

## Consuming Messages

- Consumers (running on EC2 instances, servers, or AWS Lambda)...
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the messages (example: insert the message into an RDS database)
- Delete the messages using the DeleteMessage API

![[Pasted image 20240222112546.png]]


### Multiple EC2 Instances Consumer

- Consumer receive and process messages in parallel
- At least once delivery
- Best-effort message ordering
- We can scale consumers horizontally to improve throughput of processing

![[Pasted image 20240222112823.png]]

### SQS with Auto Scaling Group(ASG)

![[Pasted image 20240222112933.png]]

## SQS to decouple between application tiers

![[Pasted image 20240222113117.png]]

## SQS - Security

### Encryption
- In-flight encryption using HTTPS API
- At-rest encryption using KMS keys
- Client-side encryption if the client wants to perform encryption/decryption itself
### Access Controls
- IAM policies to regulate access to SQS API
### SQS Access Policies
- Useful for cross-account access to SQS queues
- Useful for allowing other services (SNS, S3, ...) to write to an SQS queue