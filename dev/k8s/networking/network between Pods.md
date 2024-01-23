## Using IP address and Env Variables

The challenge now is to figure out what IP address the `auth-service` provides. We can try to apply the `auth-service.yaml` config and run `kubectl get services` and acquire `CLUSTER-IP` which is the IP address to be used within the Cluster.

So we can do this manually by running:
```bash
$ kubectl -f ./k8s/auth-service.yaml
$ kubectl get servies

NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
auth-service    ClusterIP      10.98.39.51      <none>        80/TCP           4h12m
kubernetes      ClusterIP      10.96.0.1        <none>        443/TCP          8d
users-service   LoadBalancer   10.105.215.152   <pending>     8080:30436/TCP   4h12m
```

`user-deployment.yaml`
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
					  value: "10.98.39.51"
```
We can get the `CLUSTER-IP` address from `auth-service` and use that IP address as a value.
Well, this is annoying, but it at least is stable and won't change.
#### There's a BETTER WAY 🙃

k8s provides an automatically generated environment variables in your programs with information about all the Services running in your Cluster.

k8s auto-generates the env variable with the name of your service name:
`auth-service.yaml`
```yaml
...
metadata:
	name: auth-service
...
```
k8s will replace all the `-` to `_` an all the letters in CAPS + `SERVICE_HOST`.
So, `auth-service` will be transformed into `AUTH_SERVICE_SERVICE_HOST`.
Therefore, wherever we have the code that sends request to another Pod via url or possibly using env variable(i.e. `process.env.AUTH_ADDRESS`) should be changed to using `AUTH_SERVICE_SERVICE_HOST`(`AUTH_SERVICE` if using `auth-service`).

Now, of course this will require that we use the same env variable in our docker compose file:
[[_example source code#Docker compose]]
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
			# AUTH_ADDRESS: auth
			AUTH_SERVICE_SERVICE_HOST: auth
	...
```

An also change use the updated env variables in the JS files:
[[_example source code#`users-api`]]
`users-api.js`
```javascript
...
app.post("/signup", async (req, res) => {
...
	try {
		const hashedPW = await axios.get(`http://${process.env.AUTH_SERVICE_SERVICE_HOST}/hashed-password/` + password);
...
});

...

app.post("/login", async (req, res) => {
...
	const response = await axios.get(
		`http://${process.env.AUTH_SERVICE_SERVICE_HOST}/token/` + hashedPassword + "/" + password
	);
	....

app.listen(8080);
```

## Using DNS

k8s by default provides a built-in service called [CoreDNS](https://coredns.io/).
Cluster internal Domain Names.
Services in Cluster automatically creates domain names which are available and known inside of the cluster for all of the Services.
The pattern is very simple: It's the **name of the Service** and **namespace** the Service is part of.

We can get the namespaces:
```bash
$ kubectl get namespaces

NAME                   STATUS   AGE
default                Active   8d
kube-node-lease        Active   8d
kube-public            Active   8d
kube-system            Active   8d
kubernetes-dashboard   Active   5d18h
```
📕 Unless we assign the namespace, it is going to be the `default`.

`users-deployment.yaml`
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
					  value: "auth-service.default"
```

`"auth-service.default"`
This is going to be the address which can be used inside of the code running in your Cluster to send request to other Services inside the Cluster.

So we can change our Node.js code back to using the `AUTH_ADDRESS` env variable name:
`users-api.js`
```javascript
...
app.post("/signup", async (req, res) => {
...
	try {
		const hashedPW = await axios.get(`http://${process.env.AUTH_ADDRESS}/hashed-password/` + password);
...
});

...

app.post("/login", async (req, res) => {
...
	const response = await axios.get(
		`http://${process.env.AUTH_ADDRESS}/token/` + hashedPassword + "/" + password
	);
	....

app.listen(8080);
```

And that's it! 🎉
