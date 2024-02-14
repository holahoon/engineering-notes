- Defines how Route 53 responds to DNS queries
- Don't get confused by the word "*Routing*"
	- It's not the same as Load balancer routing which routes the traffic
	- DNS does not route any traffic, it only responds to the DNS queries. DNS just helps translate hostnames into actual endpoints that the clients can use
- Route 53 supports the following Routing Policies
	- **Simple**: Used for a single resource that performs a specific function.
	- **Weighted**: Useful if you have multiple resources and you want to direct a certain percentage of traffic to each.
	- **Failover**:  Used when you want to create an active/passive setup. For instance, you might want your primary resource to serve all your traffic, but if it fails, you can reroute traffic to a backup resource.
	- **Latency based**: Allows to route traffic based on the lowest network latency for your user (i.e., which region will give them the fastest response time).
	- **Geolocation**: Routes traffic based on the geographic location of your users.
	- **Multi-Value Answer**: Used when you want Route 53 to respond to DNS queries with up to eight healthy records selected at random.
	- **Geoproximity (using Route 53 Traffic Flow feature)**: Route traffic based on the geographic location of your resources and, optionally, shift traffic from resources in one location to resources in another.

## Routing Policies - Simple

- Typically, route traffic to a single resource
- Can specify multiple values in the same record
- **If multiple values are returned, a random one is chosen by the client**
- When Alias enabled, specify only one AWS resource
- Can’t be associated with Health Checks

![[Pasted image 20240214142708.png]]

## Routing Policies - Weighted

- Control the % of the requests that go to each specific resource
- Assign each record a relative weight:
	- 𝑡𝑟𝑎𝑓𝑓𝑖𝑐(%)= *Weight for a specific record* / *sum of all the weights for all records*
	- Weights don’t need to sum up to 100
- DNS records must have the same name and type
- Can be associated with Health Checks
- Use cases: load balancing between regions, testing new application versions...
- **Assign a weight of 0 to a record to stop sending traffic to a resource**
- **If all records have weight of 0, then all records will be returned equally**

![[Pasted image 20240214142931.png]]

## Routing Policies - Latency

- Redirect to the resource that has the least latency close to us
- Super helpful when latency for users is a priority
- **Latency is based on traffic between users and AWS Regions**
- Germany users may be directed to the US (if that’s the lowest latency)
- Can be associated with Health Checks (has a failover capability)

![[Pasted image 20240214143256.png]]

## [[Health Checks]]

## Routing Policies - Failover (Active-Passive)

![[Pasted image 20240214152350.png]]
Route 53 does Health Check on the primary EC2 Instance, and if it is not healthy, it failovers and checks the secondary EC2 Instance and sends back the result instead.

## Routing Policies - Geolocation

- Different from Latency-based!
- **This routing is based on user location**
- Specify location by Continent, Country or by US State (if there’s overlapping, most precise location selected)
- Should create a “**Default**” record (in case there’s no match on location)
- Use cases: website localization, restrict content distribution, load balancing, ...
- Can be associated with Health Checks

![[Pasted image 20240214153118.png]]

## Routing Policies – Geoproximity

- Route traffic to your resources based on the geographic location of users and resources
- Ability to **shift more traffic to resources based** on the defined **bias**
- To change the size of the geographic region, specify **bias** values:
	- To expand (1 to 99) – more traffic to the resource  
	- To shrink (-1 to -99) – less traffic to the resource
- Resources can be:  
	- AWS resources (specify AWS region)
	- Non-AWS resources (specify Latitude and Longitude)
- You must use Route 53 Traffic Flow to use this feature

![[Pasted image 20240214154113.png]]
Bias is set to 0 in both regions. There basically will be a line in the middle dividing equally.

![[Pasted image 20240214154316.png]]
us-east-1 has bias set to 50, this makes the line to shift more to the left which results shift more traffic to this region == more users in us-east-1.

So, in summary, **Geoproxmitiy routing is helpful when you need to shift traffic from one region to another by increasing the bias.**

## Routing Policies - IP-based Routing

- **outing is based on clients’ IP addresses**
- **You provide a list of CIDRs for your clients** and the corresponding endpoints/locations (user-IP-to-endpoint mappings)
- Use cases: Optimize performance, reduce network costs...
- Example: route end users from a particular ISP to a specific endpoint

![[Pasted image 20240214163554.png]]

## Routing Policies - Multi-Value

- Use when routing traffic to multiple resources
- Route 53 return multiple values / resources
- Can be associated with Health Checks (return only values for healthy resources)
- Up to 8 healthy records are returned for each Multi-Value query
- **Multi-Values is not a substitute for having an ELB**

![[Pasted image 20240214164618.png]]
