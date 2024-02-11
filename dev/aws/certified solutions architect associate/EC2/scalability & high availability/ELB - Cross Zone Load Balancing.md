## Cross-Zone Load Balancing

#### With Cross Zone Load Balancing
![[Pasted image 20240210193045.png]]
Distributing the traffic evenly across all EC2 instances.

#### Without Cross Zone Load Balancing
![[Pasted image 20240210193055.png]]
The traffic is contained within each AZ, and evenly distributed within it. If there are in-balanced number of instances, some will receive slightly more traffic than other.

### Application Load Balancer
- Enabled by default (can be disabled at the Target Group level)
- No charges for inter AZ data
### Network Load Balancer & Gateway Load Balancer
- Disabled by default
- You page charges ($) for inter AZ data if enabled
### Class Load Balancer
- Disabled by default
- No charged for inter AZ data if enabled

