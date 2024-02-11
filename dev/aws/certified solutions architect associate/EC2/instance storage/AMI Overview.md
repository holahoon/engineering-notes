AMI = Amazon Machine Image. This powers the EC2 instances.

- AMI are a **customization** of an EC2 instance
	- You add your own software, configuration, operating system, monitoring...
	- Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a **specific region** (and can be copied across regions)
- You can launch EC2 instances from:
	- **A Public AMI**: AWS provided
	- **Your own AMI**: you make and maintain them yourself
	- **An AWS Marketplace AMI**: an AMI someone else made (and potentially sells)

## AMI Process (from an EC2 instance)
- Start an EC2 instance and customize it  
- Stop the instance (for data integrity)  
- Build an AMI – this will also create EBS snapshots
- Launch instances from other AMIs
![[Pasted image 20240210104140.png]]

## Hands on

### Create an AMI Image
![[Pasted image 20240210111015.png]]

Go to `Images` / `AMIs`
It takes a little while to be available. Be patient. Once it is available, You can click 
![[Pasted image 20240210111432.png]]
To launch an instance based on the custom AMI.
![[Pasted image 20240210111457.png]]
