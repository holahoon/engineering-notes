## Amazon S3 - Encryption in transit (SSL/TLS)

- Encryption in flight is also called SSL/TLS
- Amazon S3 exposes two endpoints:
	- **HTTP Endpoint** – non encrypted
	- **HTTPS Endpoint** – encryption in flight
- **HTTPS is recommended**
- **HTTPS is mandatory for SSE-C**
- Most clients would use the HTTPS endpoint by default

## Force Encryption in Transit aws:SecureTransport

![[Pasted image 20240220152808.png]]

## Default Encryption vs. Bucket Policies

- **SSE-S3 encryption is automatically applied to new objects stored in S3 bucket**
- Optionally, you can "force encryption" using bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)
![[Pasted image 20240220162444.png]]

- **Note: Bucket Policies are evaluated before "Default Encryption"**