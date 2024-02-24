## EC2 Launch Type

- ECS = Elastic Container Service
- Launch Docker containers on AWS = Launch **ECS Tasks** on ECS Clusters
- **EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances)**
- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster

![[Pasted image 20240224162423.png]]

## Fargate Launch Type

- Launch Docker containers on AWS
- **You do not provision the infrastructure (no EC2 instance to manage)**
- **It's all Serverless!**
- You just create task definitions
- AWS just runs ECS Tasks for you based on the CPU / RAM you need
- To scale, just increase the number of tasks. Simple - no more EC2 instance

![[Pasted image 20240224162803.png]]

## IAM Roles for ECS

- **EC2 Instance Profile (EC2 Launch Type only):**
	- Used by the **ECS agent**
	- Makes API calls to ECS service
	- Send container logs to CloudWatch Logs
	- Pull Docker image from ECR
	- Reference sensitive data in Secrets Manager or SSM Parameter Store
- **ECS Task Role:**
	- Allows each task to have a specific role
	- Use different roles for the different ECS Services you run
	- Task Role is defined in the **task definition**

![[Pasted image 20240224164623.png]]

## Load Balancer Integrations

- **Application Load Balancer** supported and works for most use cases
- **Network Load Balancer** recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link
- **Elastic Load Balancer** supported but not recommended (no advanced features - no Fargate)

![[Pasted image 20240224164937.png]]

## Data Volumes (EFS)

- Mount EFS files systems onto ECS tasks
- Works for both **EC2** and **Fargate** launch types
- Tasks running in any AZ will share the same data in the EFS file system
- **Fargate + EFS = Serverless**
- Use cases: persistent multi-AZ shared storage for your containers
- Note:
	- AWS S3 cannot be mounted as a file system

![[Pasted image 20240224165705.png]]
