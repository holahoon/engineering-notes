- Network load balancers (Layer 4) allow to:
	- **Forward TCP & UDP traffic to your instances**
	- Handle millions of request per seconds
	- Less latency ~100 ms (vs 400 ms for ALB)
- NLB has **one static IP per AZ**, and supports assigning Elastic IP (helpful for whitelisting specific IP)
- NLB are used for extreme performance,TCP or UDP traffic
- Not included in the AWS free tier

## TCP (Layer 4) Based Traffic
![[Pasted image 20240210183719.png]]

## Target Groups
- **EC2 instances**
- **IP Addresses** - must be private IPs
- **Application Load Balancer**
- Health Checks support the **TCP, HTTP and HTTPS Protocols**

![[Pasted image 20240210183820.png]]
