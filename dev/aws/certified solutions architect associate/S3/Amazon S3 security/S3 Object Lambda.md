
- Use AWS Lambda Functions to change the object before it is retrieved by the caller application
- Only one S3 bucket is needed, on top of which we create **S3 Access Point and S3 Object Lambda Access Points**.
- Use Cases:
    - Redacting personally identifiable information for analytics or non- production environments.
    - Convertingacrossdataformats,such as converting XML to JSON.
    - Resizing and watermarking images on the fly using caller-specific details, such as the user who requested the object.

![[Pasted image 20240220203126.png]]
