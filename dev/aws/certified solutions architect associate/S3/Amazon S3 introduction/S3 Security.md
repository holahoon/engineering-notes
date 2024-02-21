- **User-Based**
	- **IAM Policies** – which API calls should be allowed for a specific user from IAM
- **Resource-Based**
	- **Bucket Policies** – bucket wide rules from the S3 console - allows cross account
	- **Object Access Control List (ACL)** – finer grain (can be disabled)  
	- **Bucket Access Control List (ACL)** – less common (can be disabled)
- **Note**: an IAM principal can access an S3 object if  
	- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
	- AND there’s no explicit DENY
- **Encryption**: encrypt objects in Amazon S3 using encryption keys

## S3 Bucket Policies

- JSON based policies
	- Resources: buckets and objects
	- Effect: Allow / Deny
	- Actions: Set of API to Allow or Deny
	- Principal: The account or user to apply the policy to
- Use S3 bucket for policy to:
	- Grant public access to the bucket
		-Force objects to be encrypted at upload
	- Grant access to another account (Cross Account)

![[Pasted image 20240218212108.png]]

### Example: Public Access - Use Bucket Policy

![[Pasted image 20240218212314.png]]

### Example: User Access to S3 - IAM permissions

![[Pasted image 20240218212331.png]]

### Example: EC2 instance access - Use IAM Roles

![[Pasted image 20240218212438.png]]

### Advanced: Cross-Account Access - Use Bucket Policy

![[Pasted image 20240218212457.png]]

## Bucket settings for Block Public Access

![[Pasted image 20240218212538.png]]

- **These settings were created to prevent company data leaks**
- If you know your bucket should never be public, leave these on
- Can be set at the account level