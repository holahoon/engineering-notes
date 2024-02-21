## Baseline Performance

- Amazon S3 automatically scales to high request rates, latency 100-200 ms
- Your application can achieve at least **3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket**.
- There are no limits to the number of prefixes in a bucket.
- Example (object path => prefix):
	btw, prefix is anything between `bucket` and `file`.
	- bucket/folder1/sub1/file => /folder1/sub1/
	- bucket/folder1/sub2/file => /folder1/sub2/
	- bucket/1/file => /1/
	- bucket/2/file => /2/
- If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD

## S3 Performance

### Multi-Part upload
- Recommended for files > 100MB, must use for files > 5 GB
- Can help parallelize uploads (speed up transfers)
![[Pasted image 20240220144320.png]]

### S3 Transfer Acceleration
- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
- Compatible with multi-part upload
![[Pasted image 20240220144432.png]]

## S3 Byte-Range Fetches

- Parallelize GETs by requesting specific byte ranges
- Better resilience in case of failures
**Can be used to speed up downloads**
![[Pasted image 20240220145017.png]]

**Can be used to retrieve only partial data (for example, the head of a file)**
![[Pasted image 20240220145049.png]]