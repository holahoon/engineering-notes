
- In real-life, the load on your websites and application can change
- In the cloud, you can create and get rid of servers very quickly
- The goal of an Auto Scaling Group (ASG) is to:  
	- **Scale out (add EC2 instances)** to match an increased load
	- **Scale in (remove EC2 instances)** to match a decreased load  
	- Ensure we have a minimum and a maximum number of EC2 instances running  
	- Automatically register new instances to a load balancer  
	- Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
- ASG are free (you only pay for the underlying EC2 instances)

![[Pasted image 20240210215241.png]]

## ASG With Load Balancer

![[Pasted image 20240210215425.png]]

## ASG Attributes

- A **Launch Template** (older “Launch Configurations” are deprecated)
	- AMI + InstanceType
	- EC2 User Data
	- EBSVolumes  
	- Security Groups
	- SSH Key Pair
	- IAM Roles for your EC2 Instances
	- Network + Subnets Information
	- Load Balancer Information
- Min Size / Max Size / Initial Capacity
- Scaling Policies

![[Pasted image 20240210215637.png]]

## Auto Scaling - CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as **Average CPU**, or a **custom metric**)
- **Metrics such as Average CPU are computed for the overall ASG instances**
- Based on the alarm:  
	- We can create scale-out policies (increase the number of instances)
	- We can create scale-in policies (decrease the number of instances)

![[Pasted image 20240210215835.png]]

## [[ASG - Dynamic Scaling Policies]]
