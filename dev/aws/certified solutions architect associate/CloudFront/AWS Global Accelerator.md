
## Global users for our application

- You have deployed an application and have global users who want to access it directly
- They go over the public internet, which can add a lot of latency due to many hops
- We wish to go as fast as possible through AWS network to minimize latency

![[Pasted image 20240221102426.png]]

## Unicast IP vs Anycast IP

### Unicast IP
- One server holds one IP address
![[Pasted image 20240221102542.png]]
### Anycast IP
- All servers hold the same IP address and the client is routed to the nearest one
![[Pasted image 20240221102555.png]]

## AWS Global Accelerator
AWS global accelerator uses the Anycast IP.
- Leverage the AWS internal network to route to your application
- **2 Anycast IP** are created for your application
- The Anycast IP send traffic directly to Edge Locations
- The Edge locations send the traffic to your application
![[Pasted image 20240221102806.png]]

- Works with **Elastic IP, EC2 instances, ALB, NLB, public or private**
- Consistent Performance
	- Intelligent routing to lowest latency and fast regional failover
	- No issue with client cache (because the IP doesn't change)
	- Internal AWS network
- Health Checks
	- Global Accelerator performs a health check of your applications
	- Helps make your application global (failover less than 1 minute for unhealthy)
	- Great for disaster recovery (thanks to the health checks)
- Security
	- Only 2 external IP need to be whitelisted
	- DDoS protection thanks yo AWS Shield

## AWS Global Accelerator vs CloudFront

- They both use the AWS global network and its edge locations around the world
- Both services integrate with AWS Shield for DDoS protection
### CloudFront
- Improves performance for both cacheable content (such as images and videos)
- Dynamic content (such as API acceleration and dynamic site delivery)
- Content is served at the edge
### Global Accelerator
- Improves performance for a wide range of applications over TCP or UDP
- Proxying packets at the edge to applications running in one or more AWS Regions
- Good fir for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP
- Good for HTTP use cases that require static IP addresses
- Good for HTTP use cases that require deterministic, fast regional failover

