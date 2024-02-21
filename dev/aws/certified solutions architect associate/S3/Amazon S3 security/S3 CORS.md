## What is CORS?

- **Cross-Origin Resource Sharing (CORS)**
- **Origin = scheme (protocol) + host (domain) + port**
	- example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
- **Web Browser** based mechanism to allow requests to other origins while visiting the main origin
- Same origin: http://example.com/app1 & http://example.com/app2
- Different origins: http://www.example.com & http://other.example.com
- The requests won’t be fulfilled unless the other origin allows for the requests, using **CORS Headers** (example: **Access-Control-Allow-Origin**)

![[Pasted image 20240220162748.png]]

## S3 - CORS

- If a client makes a cross-origin request on our S3 bucket, we need to enable the correct CORS headers
- It’s a popular exam question  
- You can allow for a specific origin or for * (all origins)

![[Pasted image 20240220170835.png]]
