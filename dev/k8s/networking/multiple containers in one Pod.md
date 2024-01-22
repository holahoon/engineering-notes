There are times when we could have multiple Docker Containers in a single Pod.

![[Pasted image 20240121150827.png]]

Say `Users API` sends some requests to `Auth API` and these Containers are within the same Pod.

Here's an example code:
[[_example source code]]

Docker Services can communicate with each other internally, so for `users-api`, just having `http://auth` is fine. `auth` is a Service defined in the `docker-compose.yaml` configuration.

The problem is that this is not compatible with Containers in a Pod. We need to set up a way for the Containers within the Pod. We can make an environment variable `AUTH_ADDRESS` and give it a value of `auth` in the `docker-compose.yaml` file and make changes to the JavaScript files as well to use that env variable.

`docker-compose.yaml`
```yaml
version: "3"
services:
...
	users:
		build: ./users-api
		ports:
			- "8080:8080"
		environment:
			AUTH_ADDRESS: auth
....
```

`users-api/users-app.js`
```javascript
...
const hashedPW = await axios.get(
	`http://${process.env.AUTH_ADDRESS}/hashed-password/` + password
);
```
`auth-api/auth-app.js`
```javascript
const response = await axios.get(
	`http://${process.env.AUTH_ADDRESS}/token/` + hashedPassword + "/" + password
);
```
Now, our Node.js application utilizes the environment variables defined in the `docker-compose.yaml` config.

## Pod internal communication

Then what about our Kubernetes Deployment?

`k8s/users-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: users-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: users
	template:
		metadata:
			labels:
				app: users
		spec:
			containers:
				- name: users
				  image: holahoon/k8s-demo-users:latest
				  env:
					- name: AUTH_ADDRESS
					  value: localhost
					  
				- name: auth
				  image: holahoon/k8s-demo-auth:latest
```

Here, we have two Containers defined in our Pod. And the `users` Container defines an environment variable `AUTH_ADDRESS` with value of `localhost`.
Kubernetes allows to utilize `localhost` for Containers to network within the same Pod.
As a side note, we don't need a separate Deployment for the `auth-app.js` since we are not exposing any ports externally, but it rather only talks to the `users-api.js` server. So, `Auth-api` is NOT public-facing.

`k8s/users-service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
	name: users-service
spec:
	selector:
		app: users
	type: LoadBalancer
	ports:
		- protocol: TCP
		  port: 8080
		  targetPort: 8080
```
