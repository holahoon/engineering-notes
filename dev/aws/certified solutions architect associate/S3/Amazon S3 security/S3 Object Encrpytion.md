- You can encrypt objects in S3 buckets using one of 4 methods
- **Server-Side Encryption (SSE)**
	- **Server-Side Encryption with Amazon S3-Managed Keys (SSE-S3) – Enabled by Default** 
		- Encrypts S3 objects using keys handled, managed, and owned by AWS
	- **Server-Side Encryption with KMS Keys stored in AWS KMS (SSE-KMS)**
		- Leverage AWS Key Management Service(AWSKMS) to manage encryption keys
	- **Server-Side Encryption with Customer-Provided Keys (SSE-C)**
		- When you want to manage your own encryption keys
- **Client-Side Encryption**
- It’s important to understand which ones are for which situation for the exam

## S3 Encryption - SSE-S3

- Encryption using keys handled, managed, and owned by AWS
- Object is encrypted server-side
- Encryption type is **AES-256**
- Must set header **"x-amz-server-side-encryption": "AES256"**
- **Enabled by default for new buckets & new objects**
![[Pasted image 20240220152104.png]]

## S3 Encryption - SSE-KMS

- Encryption using keys handled and managed by AWS KMS (Key Management Service)
- KMS advantages: user control + audit key usage using CloudTrail
- Object is encrypted server side
- Must set header **"x-amz-server-side-encryption": "aws:kms"**
![[Pasted image 20240220152211.png]]

### SSE-KMS Limitation

- If you use SSE-KMS, you may be impacted by the KMS limits
- When you upload, it calls the **GenerateDataKey** KMS API
- When you download, it calls the **Decrypt** KMS API
- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
- You can request a quota increase using the Service Quotas Console
![[Pasted image 20240220152305.png]]

## S3 Encryption - SSE-C

- Server-Side Encryption using keys fully managed by the customer outside of AWS
- Amazon S3 does NOT store the encryption key you provide  
- HTTPS must be used
- Encryption key must provided in HTTP headers, for every HTTP request made
![[Pasted image 20240220152447.png]]

## S3 Encryption - Client-Side Encryption

- Use client libraries such as Amazon S3 Client-Side Encryption Library
- Clients must encrypt data themselves before sending to Amazon S3  
- Clients must decrypt data themselves when retrieving from Amazon S3
- Customer fully manages the keys and encryption cycle
![[Pasted image 20240220152535.png]]
