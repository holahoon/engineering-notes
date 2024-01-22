What if we want to have a Pod to Pod communication?
We are now going to have a separate `Auth-api` Pod which is NOT going to be public-facing, should be Cluster internal, and it should it be reachable by both the `Users-api` and `Tasks-api`.

![[Pasted image 20240121151907.png]]
![[Pasted image 20240121151955.png]]

Take a look at the example project code [[_example source code]]

## Deployments
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
```

`k8s/auth-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: auth-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: auth
	template:
		metadata:
			labels:
				app: auth
		spec:
			containers:
				- name: auth
				  image: holahoon/k8s-demo-auth:latest
```

## Services
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

`k8s/auth-service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
	name: auth-service
spec:
	selector:
		app: auth
	type: ClusterIP
	ports:
		- protocol: TCP
		  port: 80
		  targetPort: 80
```
We need a Service for the `Auth-api` because the `Auth-api` Pod could restart due to some errors which will change its IP address. We wouldn't know which IP address to talk to if IP addresses change.
Since Services have a stable IP address, we need to have a Service even for a Pod that is not exposed publicly.
- `ClusterIP`: Does some automatic load balancing by k8s, but it is not exposed to the outside world.

## Network between Pods via IP address and Env Variables

The challenge now is to figure out what IP address the `auth-service` provides. We can try to apply the `auth-service.yaml` config and run `kubectl get services` and acquire `CLUSTER-IP` which is the IP address to be used within the Cluster.

So we can do this manually by running:
```bash
$ kubectl -f ./k8s/auth-service.yaml
$ kubectl get servies

NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
auth-deployment-service   ClusterIP      10.105.195.80   <none>        80/TCP           6s
kubernetes                ClusterIP      10.96.0.1       <none>        443/TCP          7d20h
story-service             LoadBalancer   10.103.87.76    <pending>     80:30902/TCP     47h
users-service             LoadBalancer   10.108.61.225   <pending>     8080:30275/TCP   114m
```

Well, this is annoying, but it at least is stable and won't change.
#### There's a BETTER WAY 🙃



