- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS
- Example: Firewalls, Intrusion Detection and Prevention Systems, Deep Packet Inspection Systems, payload manipulation, ...
- Operates at Layer 3 (Network Layer) – IP Packets
- Combines the following functions:
	- **Transparent Network Gateway** – single
	- **Load Balancer** – distributes traffic to your virtual appliances
- Uses the **GENEVE** protocol on port **6081**
![[Pasted image 20240210184952.png]]

## Target Groups
- **EC2 instances**
- **IP Addresses** - must be private IPs
![[Pasted image 20240210185329.png]]
