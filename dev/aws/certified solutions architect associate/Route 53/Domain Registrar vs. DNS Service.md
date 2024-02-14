
- You buy or register your domain name with a Domain Registrar typically by paying annual charged (e.g., GoDaddy, Amazon Registrar Inc, ...)
- The Domain Registrar usually provides you with a DNS service to manage your DNS records
- But you can use another DNS service to manage your DNS records
- Example: purchase the domain from GoDaddy and use Route 53 to manage your DNS records

![[Pasted image 20240214165152.png]]

## GoDaddy as Registrar & Route 53 as DNS Service

![[Pasted image 20240214165223.png]]

## 3rd Party Registrar with Amazon Route 53

- **If you buy your domain on a 3rd party registrar, you can still use Route 53 as the DNS Service provider**
1. Create a Hosted Zone in Route 53
2. Update DNS Records on 3rd party website to use Route 53 **Name Servers**

- **Domain Registrar != DNS Service**
- Buy every Domain Registrar usually comes with some DNS features