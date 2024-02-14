
AWS Resources (Load Balancer, CloudFront...) expose an AWS hostname:
- lb1-1234.us-east-2.elb.amazonaws.com and you want myapp.mydomain.com
### CNAME:
- Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com)
- **ONLY FOR NON ROOT DOMAIN** (aka. something.mydomain.com)
### Alias:
- Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
- **Works for ROOT DOMAIN and NON ROOT DOMAIN** (aka mydomain.com)
- Free of charge
- Native health check

## Alias Records

- Maps a hostname to an AWS resource
- An extension to DNS functionality
- Automatically recognizes changes in the resource’s IP addresses
- Unlike CNAME, it can be used for the top node of a DNS namespace (Zone Apex), e.g.: example.com
- Alias Record is always of type A/AAAA for AWS resources (IPv4 / IPv6)
- **You can’t set the TTL**

![[Pasted image 20240214141343.png]]

### Alias Records Targets
- Elastic Load Balancer
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- VPC Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone
- **You CANNOT set an ALIAS record for an EC2 DNS name**

![[Pasted image 20240214141605.png]]
