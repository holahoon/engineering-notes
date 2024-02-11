- Application load balancers is Layer 7 (HTTP)
- Load balancing to multiple HTTP applications across machines (target groups)
- Load balancing to multiple applications on the same machine (ex: containers)
- Suppor t for HTTP/2 and WebSocket
- Support redirects (from HTTP to HTTPS for example)

- Routing tables to different target groups:
	- Routing based on path in URL (example.com/users & example.com/posts)
	- Routing based on hostname in URL (one.example.com & other.example.com)
	- Routing based on Query String, Headers (example.com/users?id=123&order=false)
- ALB are a great fit for micro services & container-based application (example: Docker & Amazon ECS)
- Has a port mapping feature to redirect to a dynamic port in ECS
- In comparison, we’d need multiple Classic Load Balancer per application

## ALB (v2) HTTP Based Traffic
![[Pasted image 20240210151922.png]]

## ALB (v2) Target Groups
- EC2 instances (can be managed by an Auto Scaling Group) – HTTP
- ECS tasks (managed by ECS itself) – HTTP  
- Lambda functions – HTTP request is translated into a JSON event
- IP Addresses – must be private IPs
- ALB can route to multiple target groups  
- Health checks are at the target group level

## ALB (v2) Query Strings / Parameters Routing
![[Pasted image 20240210152114.png]]

## Good to know
- Fixed hostname (XXX.region.elb.amazonaws.com)
- The application servers don’t see the IP of the client directly  
	- The true IP of the client is inserted in the header X-Forwarded-For 
	- We can also get Port (X-Forwarded-Port) and proto (X-Forwarded-Proto)
![[Pasted image 20240210152649.png]]

## Hands on
Let's create an Application Load Balancer!

### Create Load Balancer
![[Pasted image 20240210155502.png]]
- Let's select Application Load Balancer and create it.
- Give it a Load balancer name (i.e. `test-alb`).
- In `Network mapping` section, Select at least two Availability Zones (you can select them all).
- Go to `Security groups` section and select.
	- If there is no security groups to use, click on `create a new security group` to create one. Give the security group a name and some description, then make sure that in `Inbound rules`, you add a rule with type `HTTP` from Anywhere (e.g. IPv4).
	- Save it, and go back to creating load balancer and choose the security group.
- In `Listeners and routing` section
	- leave the protocol as `HTTP`, and default the Port to `80`.
	- Give it a default action. If no target group exists, click on `Create target group`.
- When Target groups tab opens, choose the `Instances` target type, give it a target group name and just leave everything as default selected (i.e. port 80, IPv4, ...)
	- Clicking `next` will guide you to `Register targets` step and here, select all the instances you want to group and click on `Include as pending below`.
	- Then finish the process by `Create target group`.
- If we go back to `Listeners and routing` section again, and refresh the list. you'll see the created target group. Select it.
- Finish `Create load balancer`.

This take a little while to create a load balancer. Once it is created, you can copy and paste the `DNS name` and try to url and try to refresh the page. If you had multiple instances, it will load balance between your instances!
If one of the instance stops or terminates, other instance will still be available!

## Only allow access through load balancer

What if we want to strict the access to the EC2 instance only through load balancer, instead of directly accessing the instance?

- Go to `Network & Security` / `Security Groups` and select the security group to edit.
- Click on `Edit inbound rules` of the selected security group and just only leave `SSH` type and delete any `HTTP` type inbound rules. This is to prevent from any inbound traffic directly to the instance.
- Click on `Add rule` and choose `HTTP` type to and select the `Source` and select the security group that we used for load balancer (from above).
- `Save rules`.
If we try to directly access the instance via public IPv4, it will just be in infinite request which means that it is not accessible 👏
We not have tightened network security 🎉









