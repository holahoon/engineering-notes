## Imperative Approach

We need to manually run `kubectl create deployment ...` and services.
Individual commands are executed to trigger certain Kubernetes actions.
It is comparable to using `docker run ...` only

Here's some examples on how to do so:
[[imperative way of working with k8s]]

## Declarative Approach

We can use command like `kubectl apply -f config.yaml`.
A config file is defined and applied to change the desired state.
It is comparable to using `Docker Compose` with compose files.

Here's some examples on how to do so:
[[declarative way of working with k8s]]



