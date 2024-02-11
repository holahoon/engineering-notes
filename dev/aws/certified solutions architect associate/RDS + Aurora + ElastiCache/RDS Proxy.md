**Fully managed database proxy for RDS**
Why do we need a proxy to access our database when we can just access it directly?
- Allows apps to pool and share DB connections established with the database
- **Improving database efficiency by reducing stress on database resources (e.g. CPU, RAM) and minimize open connections (and timeouts)**
- Serverless, autoscaling, highly available (multi-AZ)
- **Reduced RDS & Aurora failover time by up 66%**
- Supports RDS (MySQL, PostgresSQL, MariaDB) and Aurora (MySQL, PostgresSQL)
- No code changes required for most apps
- **Enforce IAM Authentication for DB, and securely store credentials in AWS Secretes Manager**
- **RDS Proxy is never publicly accessible (must be accessed from VPC)**

![[Pasted image 20240211143813.png]]
