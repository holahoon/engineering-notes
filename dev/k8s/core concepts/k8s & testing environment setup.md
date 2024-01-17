## Your Work / Kubernetes' Work
### What you need to do / setup
- Create the Cluster and the Node Instance (Worker + Master Nodes)
- Setup API Server, kubelet and other Kubernetes services / software on Nodes
- Create other (cloud) provider resources that might be needed (e.g. Load Balancer, Filesystems)
### What k8s will do
- Create your objects (e.g. Pods) and manage them
- Monitor Pods and re-create them, scale Pods etc.
- Kubernetes utilizes the provided (cloud) resources to apply your configuration / goals

## Install `kubectl` and `minikube`

We can go to the official [k8s documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/) to install `kubectl` but here's what I did to set up both `kubectl` and `minikue` using `docker` as a driver.

```bash
$ brew install kubectl
$ kubectl version --client
```
- installs `kubectl`
- checks the client version

```bash
$ brew install minikube
$ minikube start --driver=docker
$ minikube status
$ minikube dashboard
```
- installs `minikube`
- uses docker driver
- checks the status of `minikube`
- opens up dashboard
