
- A highly available, scalable, fully managed and Authoritative DNS  
	- Authoritative = the customer (you) can update the DNS records
- Route 53 is also a Domain Registrar
- Ability to check the health of your resources
- The only AWS service which provides 100% availability SLA
- Why Route 53? 53 is a reference to the traditional DNS port

![[Pasted image 20240212194213.png]]

## Records

- How you want to route traffic for a domain
- Each record contains:  
	- **Domain/subdomain Name** – e.g., example.com  
	- **Record Type** – e.g., A or AAAA  
	- **Value** – e.g., 12.34.56.78  
	- **Routing Policy** – how Route 53 responds to queries  
	- **TTL** – amount of time the record cached at DNS Resolvers
- Route 53 supports the following DNS record types:
	- (must know)A / AAAA / CNAME / NS
	- (advanced)CAA/DS/MX/NAPTR/PTR/SOA/TXT/SPF/SRV

## Record Types

- **A** – maps a hostname to IPv4
- **AAAA** – maps a hostname to IPv6
- **CNAME** – maps a hostname to another hostname
	- The target is a domain name which must have an A or AAAA record
	- Can’t create a CNAME record for the top node of a DNS namespace (Zone Apex)
	- Example: you can’t create for example.com, but you can create for www.example.com
- **NS** – Name Servers for the Hosted Zone
	- Control how traffic is routed for a domain

## Hosted Zones
A **Hosted Zone** in AWS Route 53 is essentially a container that holds information about how you want to route traffic on the internet for a specific domain, such as example.com. Each hosted zone is associated with a set of DNS records, which control the flow of traffic for that domain. AWS Route 53 automatically creates a record set that includes a name server (NS) record and a start of authority (SOA) record when you create a hosted zone. These records provide necessary information about your domain to the DNS system, establishing the basis for routing traffic for that domain to the appropriate IP address in your AWS environment.

- A container for records that define how to route traffic to a domain and its subdomains
- **Public Hosted Zones** – contains records that specify how to route traffic on the Internet (public domain names) `application1.mypublicdomain.com`
- **Private Hosted Zones** – contain records that specify how you route traffic within one or more VPCs (private domain names) `application1.company.internal`
- You pay $0.50 per month per hosted zone

## Public vs Private Hosted Zones

### Public Hosted Zone

![[Pasted image 20240212195250.png]]
It can answer any public queries from the clients (anywhere in the internet).
### Private Hosted Zone

![[Pasted image 20240212195307.png]]
Only queried within the private resources (i.e. VPC).




