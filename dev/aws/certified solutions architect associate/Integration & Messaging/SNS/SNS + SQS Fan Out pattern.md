The idea is to send message to multiple SQS. If sending individually, there could be problems (e.g., if the application crashes in between or if there are delivery failures).

![[Pasted image 20240223112343.png]]

- Push once in SNS, receive in all SQS queues that are subscribers
- Fully decoupled, no data loss
- SQS allows for: data persistence, delayed processing and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue **access policy** allows for SNS to write
- Cross-Region Delivery: works with sQS Queues in other regions

## Application
### S3 Events to multiple queues

- For the same combination of: **event type** (e.g. object create) and prefix (e.g. images/) you can only have one S3 Event rule
- If you want to send the same S3 event to many SQS queues, use fan-out

![[Pasted image 20240223112606.png]]

### SNS to Amazon S3 through Kinesis Data Firehose

- SNS can send Kinesis and therefore we can have the following solutions architecture:

![[Pasted image 20240223112729.png]]

## Amazon SNS - FIFO Topic

- FIFO = First In First Out (ordering of messages in the topic)
- Similar to features as SQS FIFO: [[SQS - FIFO Queue]]
	- **Ordering** by Message Group ID (all messages in the same group are ordered)
	- **Deduplication** using a Deduplication ID or Content Based Deduplication
- **Can have SQS Standard and FIFO queues as subscribers**
- Limited throughput (same throughput as SQS FIFO)
![[Pasted image 20240223113004.png]]

### SNS FIFO + SQS FIFO: Fan Out pattern

- In case you need to fan out + ordering + deduplication
![[Pasted image 20240223113128.png]]

## SNS - Message Filtering

- JSON policy used to filter messages sent to SNS topic's subscriptions
- If a subscription doesn't have a filter policy, it receives every message

![[Pasted image 20240223113304.png]]
