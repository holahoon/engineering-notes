- S3 can host static websites and have them accessible on the Internet
- The website URL will be (depending on the region)  
	- http://bucket-name.s3-website-aws-region.amazonaws.com
		OR
	- http://bucket-name.s3-website.aws-region.amazonaws.com
- If you get a **403 Forbidden** error, make sure the bucket policy allows public reads!
