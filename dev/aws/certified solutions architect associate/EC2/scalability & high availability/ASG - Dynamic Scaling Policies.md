## Dynamic Scaling Policies
### Target Tracking Scaling
- Most simple and easy to set-up
- Example: I want to average ASG CPU to stay around 40%
### Simple / Step Scaling
- When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
- When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
### Scheduled Actions
- Anticipate a scaling based on known usage patterns
- Example: increase the min capacity to 10 at 5pm on Fridays

## Predictive Scaling
- **Predictive scaling**: continuously forecast load and schedule scaling ahead
![[Pasted image 20240210224535.png]]

## Good metrics to scale on

- **CPUUtilization**: Average CPU utilization across your instances
- **RequestCountPerTarget**: to make sure the number of requests per EC2 instances is stable
- Average Network In / Out (if you’re application is network bound)
- Any custom metric (that you push using CloudWatch)

![[Pasted image 20240210224635.png]]

## Scaling Cooldowns
- After a scaling activity happens, you are in the **cooldown period (default 300 seconds)**
- During the cooldown period, the ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
- Advice: Use a ready-to-use AMI to reduce configuration time in order to be serving request faster and reduce the cooldown period

![[Pasted image 20240210230255.png]]

