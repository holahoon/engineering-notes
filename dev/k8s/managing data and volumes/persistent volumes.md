[documentation](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

Pod and Node independent. The idea is to have "new resources/entities" inside the cluster detached from Nodes and Pods. Instead, we can create "PV Claim" which are part of the Pods and they reach out to the Persistent Volumes to request access to the data. Of course we can have claims to multiple different Persistent Volumes. There is full flexibility.

Let's create an example of a Persistent Volume. In this example, we are just going to use `hostPath` since we only work with one Node. But, besides the `hostPath`, everything else applies the same to all the other volume types.

`host-pv.yaml`
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
	name: host-pv
spec:
	capacity:
		storage: 1Gi
	volumeMode: Filesystem
	accessModes:
		- ReadWriteOnce
		# - ReadOnlyMany
		# - ReadWriteMany
	hostPath:
		path: /app
		type: DirectoryOrCreate
```

- `kind`: `PersistentVolume` is the type of volume we are creating.
- `volumeMode`: There are two available storage modes: `Filesystem` and `Block`. [differences](https://www.computerweekly.com/feature/Storage-pros-and-cons-Block-vs-file-vs-object-storage).
- `accessMode`: How it may be accessed. How the volume is claimed for a given Pod. [docs](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes)
	- `ReadWriteOnce`: This volume can be mounted as a read & write volume by a single Node. By multiple Pods, but they all must be on the same Node.
	- `ReadOnlyMany`: Read only, but it can be claimed by multiple Nodes. So, multiple Pods from multiple Nodes can claim this PV volume.
	- `ReadWriteMany`: Same as `ReadOnlyMany`, but with write access.
	Our case of using `hostPath`, the `ReadOnlyMany` and `ReadWriteMany` will NOT be available since `hostPath` only works with one Node.
- `spec`:
	- `capacity`: As admin, we need to control how much capacity can be used by different Pods which are later executed in the cluster. [resource model](https://github.com/kubernetes/design-proposals-archive/blob/main/scheduling/resources.md#resource-specifications). In our example, up to 1 GB of space can be claimed and used by the `hostPath` volume.
	- `hostPath`: Type of volumes to use.
		- `path`, `type` these examples can be found in [[hostPath volume]]

Now, this standalone `hostPath` Persistent Volume can be used by any Pod in our application. So if we have multiple Deployments, all the Pods from multiple Deployments will have access to the volume.
But, in order to use the volume, the Pods need to use `persistent volume claim`.

## Persistent Volume Claim

`host-pv-claim.yaml`
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
	name: host-pv-claim
spec:
	volumeName: host-pv
	accessModes:
		- ReadWriteOnce
	resources:
		requests:
			storage: 1Gi
```

- `spec`:
	- `volumeName`: The name of the volume we want to claim (in our case, `host-pv` from above).
		There are also ways to claim by resource which is called "[Dynamic Volume Provisioning](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/)", this is more of an advanced administrator task. Our case of targeting by name is called "Static Provisioning".
	- `accessModes`: The modes to use for claiming the volume. 
	- `resources`: The counter part to the `capacity`. Specifies which resources to get for this claim. We have 1GB of capacity available for our Persistent Volume (`host-pv`), so our `host-pv-claim` must request storage capacity less than or 1GB.

We now need to specify the Deployment that we are going to use our claim:
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
				  persistentVolumeClaim:
					  claimName: host-pv-claim
```

- `persistentVolumeClaim`: Specifies the claim we are going to use to connect to the Persistent Volume.
	- `claimName`: The name of the claim to use.

Up to this point, the data is now fully independent from Pods. It is also theoretically independent from the Node as well.

## Storage Class

Storage Class gives fine-grain control over how storages are managed and how volumes can be configured. We can use the default storage class, but there are advanced concepts as well.

We can take a look at the Storage Class (the default SC of minikube):
```bash
$ kubectl get sc

NAME                 PROVISIONER                RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
standard (default)   k8s.io/minikube-hostpath   Delete          Immediate           false                  6d15h
```

We need to make sure that we are using the Storage Class in our Persistent Volume configuration.

`host-pv.yaml`
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
	name: host-pv
spec:
	capacity:
		storage: 1Gi
	volumeMode: Filesystem
	storageClassName: standard
	accessModes:
	...
```
We can use the name `standard` which is the default name of the SC provided by minikube.

We also need to use that same SC name when making a claim:
`host-pv-claim.yaml`
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
	name: host-pv-claim
spec:
	volumeName: host-pv
	storageClassName: standard
	accessModes:
	...
```

## Check Persistent Volume

We can now check to see if we have Persistent Volume created:
```bash
# apply the changes
$ kubectl apply -f=host-pv.yaml -f=host-pv-claim.yaml -f=deployment.yaml

# check Persistent Volumes
$ kubectl get pv
```

We should be able to see:
```
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   REASON   AGE
host-pv   1Gi        RWO            Retain           Bound    default/host-pv-claim   standard                53s
```

And if we take a look at the Deployments:
```bash
$ kubectl get deployments

NAME               READY   UP-TO-DATE   AVAILABLE   AGE
story-deployment   2/2     2            2           19h
```
We should see two up-and-running Pods.

## Summary

From `deployment.yaml` configuration, we specify that our Pod should reach out to the `host-pv-claim` claim via `persistentVolumeClaim`. All the configuration is in the `host-pv-claim` claim, and specifically in the `host-pv` configuration Volume.
The Volume is now independent from Pod and Node 🎉.

