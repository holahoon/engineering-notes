
- In case you update the backend origin, CloudFront doesn't know about it and will only get the refreshed content after the TTL has expired
- However, you can force an entire or partial cache refresh (thus bypassing the TTL) by performing a CloudFront Invalidation
- You can invalidate all file (*) or a special path (/images/*)

![[Pasted image 20240221102151.png]]
