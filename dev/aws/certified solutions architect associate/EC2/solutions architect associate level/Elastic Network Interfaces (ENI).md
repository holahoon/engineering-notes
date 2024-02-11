
ENI is a vital component of AWS infrastructure. It is designed to provide high-level network performance and security for your AWS instances.
An ENI can be thought of as a virtual network interface card that allows you to connect your instances to a Virtual Private Cloud(VPC) securely.

![[Pasted image 20240209211131.png]]

- Logical component in a VPC that represents a **virtual network card** (they are what gives EC2 instances access to the network)
- The ENI can have the following attributes:
	- Primary private IPv4, one or more secondary IPv4
	- One Elastic IP (IPv4) per private IPv4  
	- One Public IPv4  
	- One or more security groups  
	- A MAC address
- You can create ENI independently and attach them on the fly (move them) on EC2 instances for failover
- Bound to a specific availability zone (AZ)

This article can help you to understand more about AWS ENI
https://aws.amazon.com/ko/blogs/aws/new-elastic-network-interfaces-in-the-virtual-private-cloud/
