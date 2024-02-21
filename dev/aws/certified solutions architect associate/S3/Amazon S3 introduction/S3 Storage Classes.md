
- Amazon S3 Standard - General Purpose  
- Amazon S3 Standard-Infrequent Access (IA)
- Amazon S3 One Zone-Infrequent Access
- Amazon S3 Glacier Instant Retrieval
- Amazon S3 Glacier Flexible Retrieval
- Amazon S3 Glacier Deep Archive
- Amazon S3 Intelligent Tiering

- Can move between classes manually or using S3 Lifecycle configurations

## Durability and Availability

**Durability:**
- High durability (99.999999999%, 11 9’s) of objects across multiple AZ
- If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
- Same for all storage classes
**Availability:**
- Measures how readily available a service is
- Varies depending on storage class
- Example: S3 standard has 99.99% availability = not available 53 minutes a year

### General Purpose

- 99.99% Availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use Cases: Big Data analytics, mobile & gaming applications, content distribution...

### Infrequent Access

- For data that is less frequently accessed, but requires rapid access when needed
- Lower cost than S3 Standard
- **Amazon S3 Standard-Infrequent Access (S3 Standard-IA)**
	- 99.9% Availability  
	- Use cases: Disaster Recovery, backups
	- **Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)**
	- High durability (99.999999999%) in a single AZ; data lost when AZ is destroyed
	- 99.5% Availability
	- Use Cases: Storing secondary backup copies of on-premises data, or data you can recreate

### Glacier Storage Classes

- Low-cost object storage meant for archiving / backup
- Pricing: price for storage + object retrieval cost
- **Amazon S3 Glacier Instant Retrieval**
	- Millisecond retrieval, great for data accessed once a quarter
	- Minimum storage duration of 90 days
- **Amazon S3 Glacier Flexible Retrieval (formerly Amazon S3 Glacier)**:
	- Expedited (1 to 5 minutes), Standard (3 to 5 hours), Bulk (5 to 12 hours) – free
	- Minimum storage duration of 90 days
- **Amazon S3 Glacier Deep Archive – for long term storage**:
	- Standard (12 hours), Bulk (48 hours)
	- Minimum storage duration of 180 days

### Intelligent-Tiering

- Small monthly monitoring and auto-tiering fee
- Moves objects automatically between Access Tiers based on usage
- There are no retrieval charges in S3 Intelligent-Tiering

- *Frequent Access tier (automatic)*: default tier
- *Infrequent Access tier (automatic)*: objects not accessed for 30 days
- *Archive Instant Access tier (automatic)*: objects not accessed for 90 days
- *Archive Access tier (optional)*: configurable from 90 days to 700+ days
- *Deep Archive Access tier (optional)*: config. from 180 days to 700+ days

## Storage Comparison

![[Pasted image 20240220093409.png]]

### Price comparison
example: **us-east-I**
![[Pasted image 20240220093448.png]]

## Moving between Storage Classes

- You can transition objects between storage classes
- For infrequently accessed object, move them to **Standard IA**
- For archive objects that you don’t need fast access to, move them to **Glacier or Glacier Deep Archive**
- Moving objects can be automated using a **Lifecycle Rules**

![[Pasted image 20240220102016.png]]

##  Lifecycle Rules

- **Transition Actions** – configure objects to transition to another storage class
	- Move objects to Standard IA class 60 days after creation
	- Move to Glacier for archiving after 6 months
- **Expiration actions** – configure objects to expire (delete) after some time
	- Access log files can be set to delete after a 365 days
	- **Can be used to delete old versions of files (if versioning is enabled)**
	- Can be used to delete incomplete Multi-Part uploads
- Rules can be created for a certain prefix (example: s3://mybucket/mp3/*)  
- Rules can be created for certain objectsTags (example:Department:Finance)

### Lifecycle Rules scenario I

- **Your application on EC2 creates images thumbnails after profile photos are uploaded to Amazon S3. These thumbnails can be easily recreated, and only need to be kept for 60 days.The source images should be able to be immediately retrieved for these 60 days, and afterwards, the user can wait up to 6 hours. How would you design this?**
    
- S3 source images can be on **Standard**, with a lifecycle configuration to transition them to **Glacier** after 60 days
- S3 thumbnails can be on **One-Zone IA**, with a lifecycle configuration to expire them (delete them) after 60 days
### Lifecycle Rules scenario II

- **A rule in your company states that you should be able to recover your deleted S3 objects immediately for 30 days, although this may happen rarely. After this time, and for up to 365 days, deleted objects should be recoverable within 48 hours.**
    
- **Enable S3 Versioning** in order to have object versions, so that “deleted objects” are in fact hidden by a “delete marker” and can be recovered
- Transition the “noncurrent versions” of the object to **Standard IA**
- Transition afterwards the “noncurrent versions” to **Glacier Deep Archive**

## S3 Analytics - Storage Class Analysis

- Help you decide when to transition objects to the right storage class
- Recommendations for **Standard** and **Standard IA**
	- Does NOT work for One-Zone IA or Glacier
- Report is updated daily
- 24 to 48 hours to start seeing data analysis
- Good first step to put together Lifecycle Rules (or improve them)!

![[Pasted image 20240220105102.png]]

