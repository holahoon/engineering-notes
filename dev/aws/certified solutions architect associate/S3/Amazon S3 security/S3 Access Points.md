
![[Pasted image 20240220202522.png]]

- Access Points simplify security management for S3 Buckets
- Each Access Point has:
	- its own DNS name (Internet Origin or VPC Origin)
	- an access point policy (similar to bucket policy) – manage security at scale

## Access Points - VPC Origin

- We can define the access point to be accessible only from within the VPC
- You must create aVPC Endpoint to access the Access Point (Gateway or Interface Endpoint)
- The VPC Endpoint Policy must allow access to the target bucket and Access Point

![[Pasted image 20240220202811.png]]
