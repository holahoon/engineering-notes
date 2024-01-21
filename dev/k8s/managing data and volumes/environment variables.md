We can also set and use environment variables!

You can find the example from [[working with an example]]

Let's create an env variable: `STORY_FOLDER` to replace `'story'` directory:
```javascript
const path = require('path');
...
// const filePath = path.join(__dirname, 'story', 'text.txt');
const filePath = path.join(__dirname, process.env.STORY_FOLDER, 'text.txt');
...
app.listen(3000);
```

Then in our Deployment configuration:
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
				  env:
					  - name: STORY_FOLDER
					    value: 'story'
				  volumeMounts:
					  - mountPath: /app/story
					    name: story-volume
			volumes:
				- name: story-volume
				  persistentVolumeClaim:
					  claimName: host-pv-claim
```

- `env`: Environment variable to externalize.
	- `name`: The name of the env variable
	- `value`: The value of the env variable. The `mountPath` has a path of `/app/story` so it should match that.
So, with the env variable, if we ever want to make changes to the directory name of `story`, we can just go to `deployment.yaml` configuration and make changes here 👏

## Environment Variables & ConfigMaps

But,,,, what if I want to manage a separate environment file? We can have all the environment variables in one file and map it with our Deployment configuration.
[documentation](https://kubernetes.io/docs/concepts/configuration/configmap/)

Let's create a yaml file(*name is up to you*):
`environment.yaml`
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
	name: data-store-env
data:
	folder: 'story'
	# key: value...
```

- `data`: Specifies key:value pair; each key maps to a simple value. Can have multiple keys.

Then in our Deployment configuration, we need to map it:
`deployment.yaml`
```yaml
...
env:
	- name: STORY_FOLDER
	  valueFrom:
		  configMapKeyRef:
			  name: data-store-env
			  key: folder
...
```

- `name`: Name of the environment variable.
- `valueFrom`: Pulls the values from.
	- `configMapKeyRef`: Reference to the environment to map with.
		- `name`: Name of the environment variable configuration.
		- `key`: The key of the environment variable (in our case, the `folder`).

Let's now apply it:
```bash
$ kubectl apply -f=environment.yaml -f=deployment.yaml
```

Let's take a look at all the config maps:
```bash
$ kubectl get configmap

# will get something like this:
NAME               DATA   AGE
data-store-env     1      12m
kube-root-ca.crt   1      6d23h
```
