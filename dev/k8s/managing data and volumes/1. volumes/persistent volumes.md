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


