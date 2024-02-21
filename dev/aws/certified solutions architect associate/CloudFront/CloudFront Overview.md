- Content Delivery Network (CDN)
- **Improves read performance, content is cached at the edge**
- Improves user experience
- 216 Point of Presence globally (edge locations)
- **DDoS protection (because worldwide), integration with Shield, AWS Web Application Firewall**

![[Pasted image 20240221093342.png]]
https://aws.amazon.com/ko/cloudfront/features/?nc=sn&loc=2&whats-new-cloudfront.sort-by=item.additionalFields.postDateTime&whats-new-cloudfront.sort-order=desc

## CloudFront - Origins

- **S3 bucket**
	- For distributing files and caching them at the edge
	- Enhanced security with CloudFront **Origin Access Control (OAC)**
	- OAC is replacing Origin Access Identity (OAI)
	- CloudFront can be used as an ingress (to upload files to S3)
- **Custom Origin (HTTP)**
	- Application Load Balancer
	- EC2 instance
	- S3 website (must first enable the bucket as a static S3 website)
	- Any HTTP backend you want

## CloudFront at a high level

![[Pasted image 20240221093639.png]]

## S3 as an Origin

![[Pasted image 20240221093739.png]]

## CloudFront vs S3 Cross Region Replication

### CloudFront:
CDN to cache content all around the world.
- Global Edge Network
- Files are cached for a TTL (maybe a day)
- **Great for static content that must be available everywhere**
### S3 Cross Region Replication:
Replicate an entire bucket into another region.
- Must be setup for each region you want replication to happen
- Files are updated in near real-time
- Read only
- **Great for dynamic content that needs to be available at low-latency in few regions**