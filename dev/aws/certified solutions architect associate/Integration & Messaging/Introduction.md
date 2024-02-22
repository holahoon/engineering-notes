
When we start deploying multiple applications, they will inevitably need to communicate with one another
- There are two patterns of application communication
 ![[Pasted image 20240222110315.png]]

- Synchronous between applications can be problematic if there are sudden spikes of traffic
- What if you need to suddenly encode 1000 videos but usually it's 10?
- In that case, it's better to **decouple** your application
	- using SQS: queue model
	- using SNS: pub/sub model
	- using Kinesis: real-time streaming model
- These services can scale independently from our application!