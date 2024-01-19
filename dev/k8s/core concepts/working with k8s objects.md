Kubernetes works with **Objects**.
- Pods
- Deployments
- Services
- Volume
- etc...
Objects can be created in two ways: **Imperatively** or **Declaratively**.

## The `Pod` Object

The smallest "unit" Kubernetes interacts with.
- The idea is that it contains and runs one or multiple containers. The most common use-case is "one container per Pod".
- Pods contain shared resources (e.g. volumes) for all Pod containers.
- Has a cluster-internal IP by default. Containers inside a Pod can communicate via localhost.
Pods are designed to be **ephemeral**: Kubernetes will start, stop and replace them as needed (lose state).
It is ideal for K8s to manage Pods for me, I just need a **"Controller"** (e.g. a "Deployment").

## The `Deployment` Object

Controls (multiple) Pods.
- You set a desired state, k8s then changes the actual state. Define which Pods and containers to run and the number of instances.
- Deployments can be paused, deleted and rolled back.
- Deployments can be scaled dynamically (and automatically). You can change the number of desired Pods as needed.
Deployments manage a Pods for you, you can also create multiple Deployments.
You therefore typically don't directly control Pods, instead you use Deployments to set up the desired end state.
