Route53 health checks enable you to monitor the health and performance of your applications, network, and servers. You can create custom health checks that verify the status of specific resources, such as a web server or email server. If the health check fails, Route 53 routes traffic away from the unhealthy resources. Health checks run periodically, at intervals that you specify, to help you detect issues before your end-users do. You can configure alarms to notify you when a resource becomes unhealthy, helping you respond rapidly to potential issues. AWS Route 53 Health Checks also integrates with CloudWatch, providing detailed metrics and graphs for analyzed data.
## Health Checks
- HTTP Health Checks are only for **public resources**
- Health Check => Automated NDS Failover:
	1. Health checks that monitor an endpoint (application, server, other AWS resource)
	2. Health checks that monitor other health checks (Calculated Health Checks)
	3. Health checks that monitor CloudWatch Alarms (full control !!) – e.g., throttles of DynamoDB, alarms on RDS, custom metrics, ... (helpful for private resources)
- Health Checks are integrated with CW metrics

![[Pasted image 20240214145005.png]]

## Monitor an Endpoint

- **About 15 global health checkers will check the endpoint health**
	- Healthy/UnhealthyThreshold–3(default)  
	- Interval – 30 sec (can set to 10 sec – higher cost)
	- Supported protocol: HTTP, HTTPS and TCP
	- If > 18% of health checkers report the endpoint is healthy, Route 53 considers it **Healthy**. Otherwise, it’s **Unhealthy**
	- Ability to choose which locations you want Route 53 to use
- Health Checks pass only when the endpoint responds with the 2xx and 3xx status codes
- Health Checks can be setup to pass / fail based on the text in the first **5120 bytes** of the response
- Configure you router/firewall to allow incoming requests from Route 53 Health Checkers

![[Pasted image 20240214145217.png]]

## Calculated Health Checks

- Combine the results of multiple Health Checks into a single Health Check
- You can use **OR**, **AND**, or **NOT**
- Can monitor up to 256 Child Health Checks
- Specify how many of the health checks need to pass to make the parent pass
- Usage: perform maintenance to your website without causing all health checks to fail

![[Pasted image 20240214145429.png]]

## Private Hosted Zones

- Route 53 health checkers are outside the VPC
- They can’t access **private** endpoints (private VPC or on-premises resource)
- You can create a **CloudWatch Metric** and associate a **CloudWatch Alarm**, then create a Health Check that checks the alarm itself

![[Pasted image 20240214145748.png]]

## 



