## Using `Deployment` to run commands

Use `kubectl` to send instructions to k8s cluster from our local machine.

Create a Deployment object:
```bash
$ kubectl create ...
```
This is the imperative approach.

Create deployment:
```bash
$ kubectl create deployment <name> --image=<image-name>
```
- `<name>`: we can give the deployment a name, (i.e. `first-app`)
- `--image=<image-name>`: image needs to be pushed to Docker Hub(if using Docker as a driver) (i.e. `--image=holahoon/k8s-first-app`)

We can now check:
```bash
# check for deployments
$ kubectl get deployments

# check for pods
$ kubectl get pods
```

We can also check the cluster:
```bash
$ minikube dashboard
```
It will open up a dashboard cluster where you can see Deployments, Pods and etc...

## Behind the scenes of `kubectl`

```bash
$ kubectl create deployment <name> --image=<image-name>
```
😎 This will create Deployment object and automatically sends it to our Kubernetes cluster, Master Node(Control Plane). Master Node creates all the things we need in the cluster and distributes the Pods across Worker Nodes. A newly created Pod(s) created based on the Deployment will then be moved to one of the Worker Nodes. On the Worker Node has kubelet that manages the Pod and Containers. The Pod contains a Container based on the Image that was specified when Deployment object was created.
Example:
```bash
$ kubectl create deployment first-app --image=holahoon/k8s-first-app
```

## The `Service` Object

Exposes Pods to the Cluster or Externally.
- Pods have an internal IP by default -- it changes when a Pod is replaced. So we can't rely on a Pod keeping its IP address. Therefore, IP is not really a great tool to communicate to that Pod even if it is Cluster internal. So, finding Pods is hard if the IP changes all the time.
- Services group Pods with a shared IP. This address won't change.
- Services can allow external access to Pods. The default (internal only) can be overwritten.
Without Services, Pods are very hard to reach and communication is difficult.
Reaching a Pod from outside the Cluster is not possible at all without Services.

## Exposing `Deployment` through `Service`

```bash
$ kubectl expose deployment <name> --type=ClusterIP --port=<port>
```
- `--port=<port>`: the port to expose (i.e. a Node application with `app.listen(8080)`, you'd want to expose the port `8080` because that's what the containerized app listens on).
- `--type=<type>`: type of Service or Exposing.
	- `ClusterIP`: the default type (only reachable from inside the Cluster).
	- `NodePort`: the Deployment should be exposed with the help of the IP address of the Worker Node of which it is running.
	- `LoadBalancer`: utilizes load balancer. This load balancer generates a unique address for this Service. And it will evenly distribute incoming traffic across all Pods which are part of this Service.
Here's an example:
```bash
$ kubectl expose deployment first-app --type=LoadBalancer --port=8080
```

We can check the Services:
```bash
$ kubectl get services
```
We should be able to see two Services running:
```
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
first-app    LoadBalancer   10.105.31.133   <pending>     8080:30957/TCP   8s
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          3d19h
```
The `EXTERNAL-IP` will always be `pending` for minikube. This is because minikube is still a virtual machine running in our local machine. It doesn't have too many IPs available.
But, minikube has a command that gives us an access to the Service:
```bash
$ minikube service <name>

# example:
$ minikube service first-app
```
This will open up in a browser 🚀

But do keep in mind that this imperative approach has couple of crucial steps.

## Manually Scale Pods

If we don't have auto-scaling in-place, but we know that we have more traffic incoming. We don't just want to have one Pod up and running once, but three times (for example).
```bash
$ kubectl scale deployment/first-app --replicas=3
```
- `--replicas`: is simply an instance of a Pod / Container. So, here, 3 Replicas means that the Pod / Container is running 3 times.
Because we have load balancer, traffic will be distributed evenly between all 3 Pods. Say one Pod goes down due to an error, traffic will be redirected to another Pod.
We can then just scale it down to 1 Pod by running `--replica=1`.

## Updating `Deployment`

When we make changes to our code, we need to rebuild a Docker Image and push it to Docker Hub.
We would need to tell k8s to use the updated Image.
After rebuilding and pushing the Image to Docker Hub,
```bash
$ kubectl set image deployment/<deployment-name> <current-container-name>=<new-container-name>
```
- `set image`: set a new image
- `deployment/first-app`: specific deployment to set
- `<current-container-name>`: current image to use to replace with
- `=<new-container-name>`: to be replaced image (in our case, it would be the image to be pulled from Docker Hub)
Here's an example:
```bash
$ kubectl set image deployment/first-app k8s-first-app/holahoon/k8s-first-app
```
So, we are saying, update our `first-app` deployment with new image and replace the `k8s-first-app` container with `holahoon/k8s-first-app` container from Docker Hub that's been updated.
You can check the name of the Pod in the dashboard by running `kubectl dashboard` in the Pod section.

💡 **Keep in mind that new Docker Images will ONLY be downloaded if they HAVE A DIFFERENT TAG.**

For example, we need to give our image a new tag:
```bash
# build new iamge with a new tag
$ docker build -t k8s-first-app:2 .

# rename the image to match Docker Hub's (this is optional if it already matches)
$ docker tag k8s-first-app:2 holahoon/k8s-first-app:2

# push the image to Docker Hub
$ docker push holahoon/k8s-first-app:2
```
Here, the tag is `2`.
It is always a good idea to give new tags to our Docker Image in production.
So with this, we can pull the updated Image from Docker Hub and set our image:
```bash
$ kubectl set image deployment/first-app k8s-first-app/holahoon/k8s-first-app:2
```

We can now check the status of the roll out:
```bash
$ kubectl rollout status deployment/first-app
```
It will print out `deployment "first-app" successfully rolled out`  it roll out was successful.

We can now see our changes 🚀

## Roll back a `Deployment`

Let's say we had a typo in our command when trying to update our `Deployment`. Say that we never build a `holahoon/k8s-first-app:3` tag so this wouldn't exist in Docker Hub.
```bash
$ kubectl set image deployment/first-app k8s-first-app/holahoon/k8s-first-app:3
```

If we look at the current Pods:
```bash
$ kubectl get pods
```
It would print something like this:
```
NAME                         READY   STATUS         RESTARTS   AGE
first-app-6668b986cb-xdf5v   1/1     Running        0          35m
first-app-6f97446dbf-dfssc   0/1     ErrImagePull   0          11s
```
The status of the non-existing Image would result an error.
We can roll back to the previous state:
```bash
$ kubectl rollout undo <deployment-name>

# example
$ kubectl rollout undo first-app
```

If wanting to go back to even older deployment:
```bash
$ kubectl rollout history <deployment-name>

# example
$ kubectl rollout history deployment/first-app
```
This will show the history of the Deployment:
```
deployment.apps/first-app
REVISION  CHANGE-CAUSE
1         <none>
3         <none>
4         <none>
```

To see more detail of a certain revision:
```bash
$ kubectl rollout history <deployment-name> --revision=<REVISION-number>

# example
$ kubectl rollout history deployment/first-app --revision=3
```

To go to the specific revision:
```bash
$ kubectl rollout history <deployment-name> --to-revision=<REVISION-number>

# example
$ kubectl rollout history deployment/first-app --to-revision=1
```
This will go to the revision 1 status Deployment.

## Delete `Deployment` and `Service`

We can delete `Deployment` or `Service` by name:
```bash
$ kubectl delete service <deploynment-name>
$ kubectl delete deployment <deploynment-name>

# example
$ kubectl delete service first-app
$ kubectl delete deployment first-app
```
