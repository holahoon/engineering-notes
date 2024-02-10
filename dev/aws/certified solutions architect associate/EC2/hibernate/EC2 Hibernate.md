We know we can stop, terminate instances 
	- **Stop** – the data on disk (EBS) is kept intact in the next start  
	- **Terminate** – any EBS volumes (root) also set-up to be destroyed is lost
On start, the following happens:
	- First start: the OS boots & the EC2 User Data script is run
	- Following starts: the OS boots up
	- Then your application starts, caches get warmed up, and that can take time!

## EC2 Hibernate

- The in-memory (RAM) state is preserved
- The instance boot is much faster! (the OS is not stopped / restarted)
- Under the hood: the RAM state is written to a file in the root EBS volume
- The root EBS volume must be encrypted

![[Pasted image 20240209221343.png]]

Use cases:
- Long-running processing
- Saving the RAM state
- Services that take time to initialize

## Good to know

- **Supported Instance Families** – C3, C4, C5, I3, M3, M4, R3, R4,T2,T3, ...  
- **Instance RAM Size** – must be less than 150 GB.  
- **Instance Size** – not supported for bare metal instances.  
- **AMI** – Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows... • Root Volume – must be EBS, encrypted, not instance store, and large
- Available for **On-Demand**, **Reserved** and **Spot** Instances
- An instance can **NOT** be hibernated more than 60 days

## Hands on

When launching an instance, go to `Advanced Details` > and enable `Stop - Hibernate behavior`.
![[Pasted image 20240209222317.png]]

Go up to `Storage (volumes)`:
![[Pasted image 20240209222214.png]]
We need to have our EBS Volumes `Encrypted` and give `KMS keys`.

### Connect the instance

Connect the instance to check if we have enabled hibernation.
In the CloudShell, type in `uptime` to see how long the instance has been on since the last restart. It may say 0 min initially, so wait like a minute or so.

### Hibernate the instance

Click on `Instance State` and hibernate instance.
![[Pasted image 20240209222815.png]]
This is going to make sure that all the data in the RAM onto our EBS volume.

After hibernating it, it will stop the instance, but if we start it back on and connect the instance, notice how the `uptime` doesn't print out 0 min anymore! This means that instance was never "stopped", but it was hibernated.
