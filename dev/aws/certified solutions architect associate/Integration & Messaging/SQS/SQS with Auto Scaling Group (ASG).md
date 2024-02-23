
The goal is to scale the ASP automatically based on the queue size

![[Pasted image 20240223110659.png]]

## If the load is too big, some transactions may be lost

![[Pasted image 20240223111010.png]]

To solve this issue,
## SQS as a buffer to database writes

![[Pasted image 20240223111110.png]]
This pattern works only if the client doesn't need to receive any kind of confirmation that the write has happened to the database.

## SQS to decouple between application tiers

![[Pasted image 20240223111308.png]]
