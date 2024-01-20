[documentation](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)

Whenever Pods are restarted, all the data via volumes will be lost.

## The "emptyDir" approach

We can add `volumes` to our deployment configuration:
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
				  emptyDir: {}
```
`volumes: ...` It receives a list of volumes. The name is up to you (`story-volume` in our case).
- `emptyDir: {}`: No specific configurations, but use default settings. It simply creates a new empty directory whenever the Pod starts and keeps the directory alive and filled with data as long as the Pod is alive. Containers can write to the directory and if containers are restarted or removed, the data survives, but when Pod is removed, this directory is also removed. When a new Pod is created, an empty directory is created as well.

`volumeMounts: ...` Binds the volume to the container. Takes a list as well.
- `mountPath`: Container internal path where the volume should be mounted. It depends on the application. If our Node application write to `const filePath = path.join(__dirname, 'story', 'text.txt)` (can be found in [[working with an example]]), it should store the data inside `/app/story`. Here, the `/app` is the `WORKDIR`(working directory) specified in the docker file ([[working with an example]]). So, `/app/story` is the path where the `emptyDir` should be mounted in.
- `name`: The `volumeMounts` > `name` is the name of the volume we want to mount. The `story-volume` volume is specified in the `volumes` > `name`.

Now, if we try the `/error` endpoint which kills the process and makes the Pod to restart, we will verify that the data still persists.

## Downside

What if we have multiple instances of the Pods(`replicas`)
```yaml
...
spec:
	replicas: 2
...
```

When one Pod is reached and therefore a request is sent to a different Pod, the data is lost.

One way of working around this is to switch away from `emptyDir` which creates a new empty directory per Pod to a different driver. The [[hostPath volume]] driver.

The `emptyDir` is useful if needing to practice or test.