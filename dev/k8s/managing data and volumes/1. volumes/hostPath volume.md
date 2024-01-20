[documentation](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath)

`hostPath` allows to setup Pods on the host node's machine(the real machine running this Pod) into your Pod. The data from that Pod will be exposed to other Pods.
`hostPath` is NOT Pod specific.

`deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: story-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: story
	template:
		metadata:
			labels:
				app: story
		spec:
			containers:
				- name: story
				  image: holahoon/k8s-story
				  volumeMounts:
					  - mountPath: /app/story
					    name: story-volume
			volumes:
				- name: story-volume
				  hostPath:
					  path: /data
					  type: DirectoryOrCreate
```

- `hostPath` > `path`: The path we want to have our data stored. This is the path on host machine. It's like bind mount in Docker where we bind a specific path on our local machine to the container internal path. Hence, the path we specify is the `/data`. The name is up to mind (as long I don't use paths reserved in the linux). So, the data in `/data` will be shared with `/app/story` path inside of the container.
- `type`: It is `DirectoryOrCreate` means that `/app` should be an existing directory, if non exists, then it creates. If just using `Directory` will fail if `/app` directory doesn't exist.

We now can apply the changes:
```bash
$ kubectl apply -f=deployment.yaml
```
So now even if one Pod fails, the data in our volume should persist.

## Downside

The hostPath is NOT Pod specific, but it is Node specific.
In the example we created, there only is one Node in our minikube local virtual. machine. But if we have a bigger cluster with multiple Worker Nodes (most of the cases), multiple Pods(replicas) running different Nodes will NOT have access to the data.

`hostPath` can be useful if data needs to be independent from other Pods.
