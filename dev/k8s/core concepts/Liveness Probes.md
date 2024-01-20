[documentation](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
Kubernetes checks the whether Pods or Containers in Pods are healthy or not.
The kubelet uses liveness probes to know when to restart a container. But, we can configure this on our own:

`deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: second-app-deployment
...

spec:
	...
	template:
		metadata:
			...
		spec:
			containers:
				- name: second-node
				  image: holahoon/some-image
				  livenessProbe:
					  httpGet:
						  path: /
						  port: 8080
					  initialDelaySeconds: 3
					  periodSeconds: 10
			
```

Say we have a Node.js application with an endpoint:
```javascript
app.get("/error", (req, res) => {
	process.exit(1);
});
```

Let's try visiting `<url>/error` to exit out.

Defining a liveness probe is useful when we have an application that doesn't react to the default. Or if you simply just want to check the health by sending a request to certain endpoint.

Here's a [Github repository](https://github.com/bhargavjagan/sample-kube-probes/tree/master) that has some example on doing this approach.