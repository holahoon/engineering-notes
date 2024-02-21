- Perform bulk operations on existing S3 objects with a single request, example:
	- Modify object metadata & properties
	- Copy objects between S3 buckets
	- **Encrypt un-encrypted objects**
	- ModifyACLs, tags
	- Restore objects from S3 Glacier
	- Invoke Lambda function to perform custom action on
- A job consists of a list of objects, the action to perform, and optional parameters
- S3 Batch Operations manages retries, tracks progress, sends completion notifications, generate reports ...
- **You can use S3 Inventory to get object list and use S3 Select to filter your objects**

![[Pasted image 20240220150314.png]]