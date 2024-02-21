
- Adopt a WORM (Write Once Read Many) model
- Create a Vault Lock Policy
- Lock the policy for future edits (can no longer be changed or deleted)
- Helpful for compliance and data retention

![[Pasted image 20240220201915.png]]

## S3 Object Lock (versioning must be enabled)

- Adopt a WORM (Write Once Read Many) model
- Block an object version deletion for a specified amount of time
- **Retention mode - Compliance:**
	- Object versions can't be overwritten or deleted by any user, including the root user
	- Objects retention modes can't be changed, and retention periods can't be shortened
- **Retention mode - Governance:**
	- Most users can't overwrite or delete an object version or alter its lock settings
	- Some users have special permissions to change the retention or delete the object
- **Retention Period:** protect the object for a fixed period, it can be extended
- **Legal Hold:**
	- protect the object indefinitely, independent from retention period
	- can be freely placed and removed using the *s3:PutObjectLegalHold* IAM permission