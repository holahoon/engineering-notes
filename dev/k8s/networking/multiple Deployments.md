What if we want to have a Pod to Pod communication?
We are now going to have a separate `Auth-api` Pod which is NOT going to be public-facing, should be Cluster internal, and it should it be reachable by both the `Users-api` and `Tasks-api`.

from
![[Pasted image 20240121151907.png]]
to
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

We can run:
```bash
$ kubectl apply -f ./k8s/auth-deployment.yaml
$ kubectl apply -f ./k8s/users-deployment.yaml
$ kubectl apply -f ./k8s/auth-service.yaml
$ kubectl apply -f ./k8s/users-service.yaml
```

```bash
$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
auth-deployment    1/1     1            1           6m24s
users-deployment   1/1     1            1           3s
```

```bash
$ kubectl get services
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
auth-service    ClusterIP      10.98.39.51      <none>        80/TCP           6m55s
kubernetes      ClusterIP      10.96.0.1        <none>        443/TCP          8d
users-service   LoadBalancer   10.105.215.152   <pending>     8080:30436/TCP   6m20s
```

We can see that we have a total of two Pods and two Services connected to the Pod.

btw, make sure to delete and re-apply if Docker image have been newly changed and pushed:
```bash
$ kubectl delete deploy <deployment-name>
$ kubectl delete service <service-name>
```

We can continue to look at more examples on how to connect multiple Pods:
[[network between Pods]]


