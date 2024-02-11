Load Balances are servers that forward traffic to multiple servers (e.g., EC2 instances) downstream
![[Pasted image 20240210142740.png]]
The more users, the more load is going to be balanced to cross EC2 instances, but the idea is that the users do NOT know which instances the backend is connected to. They just know that they have to connect to the load balancer which gives them one endpoint of connectivity only.

## Why use a load balancer?
- Spread load across multiple downstream instances  
- Expose a single point of access (DNS) to your application
- Seamlessly handle failures of downstream instances  
- Do regular health checks to your instances  
- Provide SSL termination (HTTPS) for your websites  
- Enforce stickiness with cookies  
- High availability across zones  
- Separate public traffic from private traffic
## Why use an Elastic Load Balancer?
- An Elastic Load Balancer is a **managed load balancer**
	- AWS guarantees that it will be working  
	- AWS takes care of upgrades, maintenance, high availability
	- AWS provides only a few configuration knobs
- It costs less to setup your own load balancer but it will be a lot more effort on your end
- It is integrated with many AWS offerings / services
	- EC2, EC2 Auto Scaling Groups, Amazon ECS  
	- AWS Certificate Manager (ACM), CloudWatch  
	- Route53, AWSWAF, AWS Global Accelerator
😎 Elastic Load balancer is a no-brainer when it comes to load balancing in AWS

## Health Checks
- Health Checks are crucial for Load Balancers
- They enable the load balancer to know if instances it forwards traffic to are available to reply to requests
- The health check is done on a port and a route (/health is common)
- If the response is not 200 (OK), then the instance is unhealthy
![[Pasted image 20240210143304.png]]

## Types of load balancer on AWS
- AWS has **4 kinds of managed Load Balancers**
- **Classic Load Balancer** (v1 - old generation) – 2009 – CLB
	- HTTP, HTTPS,TCP,SSL(secureTCP)
- **Application Load Balancer** (v2 - new generation) – 2016 – ALB
	- HTTP, HTTPS,WebSocket
- **Network Load Balancer** (v2 - new generation) – 2017 – NLB
	- TCP,TLS(secureTCP),UDP
- **Gateway Load Balancer** – 2020 – GWLB
	- Operates at layer 3 (Network layer) – IP Protocol
- Overall, it is recommended to use the newer generation load balancers as they provide more features
- Some load balancers can be setup as **internal** (private) or **external** (public) ELBs

## Load Balancer Security Groups
![[Pasted image 20240210143547.png]]
