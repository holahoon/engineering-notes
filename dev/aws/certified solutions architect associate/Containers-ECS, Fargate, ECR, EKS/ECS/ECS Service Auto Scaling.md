
- Automatically increase/decrease the desired number of ECS tasks
- Amazon ECS Auto Scaling uses **AWS Application Auto Scaling**
	- ECS Service Average CPU Utilization
	- ECS Service Average Memory Utilization - Scale on RAM
	- ALB Request Count Per Target - metric coming from the ALB

- **Target Tracking** - scale based on target value for a specific CloudWatch metric
- **Step Scaling** - scale based on a specified CloudWatch Alarm
- **Scheduled Scaling** - scale based on a specified date/time (predictable changes)

- ECS Service Auto Scaling (task level) **!=** EC2 Auto Scaling (EC2 instance level)
- Fargate Auto Scaling is much easier to setup (because **Serverless**)

## Auto Scaling EC2 Instances

- Accommodate ECS Service Scaling by adding underlying EC2 Instances
- **Auto Scaling Group Scaling**
	- Scale your ASG based on CPU Utilization
	- Add EC2 instances over time
- **ECS Cluster Capacity Provider** (*recommended way*)
	- Used to automatically provision and scale the infrastructure for your ECS Tasks
	- Capacity Provider paired with an Auto Scaling Group
	- Add EC2 Instances when you're missing capacity (CPU, RAM, ...)

### Service CPU Usage Example

![[Pasted image 20240224171422.png]]


