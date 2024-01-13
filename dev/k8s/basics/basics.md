Kubernetes, also known as K8s, is an open-source system for automating deployment, scaling, and management of containerized applications.

## Problem of manual deployments

Manual deployment of Containers is hard to maintain, error-prone and annoying.
- There are times when our containers might go down/crash and needs to be replaced
- We might even need more containers instances upon traffic spikes
- Incoming traffic should be distributed equally.

## Why K8S?

- Using a specific cloud service locks us into that that service
- You would need to learn about the specifics, services and config options of another provider if you want to switch

## What exactly is Kubernetes?

### It is NOT
- a cloud service provider
- a service by a cloud service provider
- restricted to any specific (cloud) service provider
- just a software you run on some machine
- an alternative to Docker
- a paid service
### It is
- an open-source project
- can be used with any provider
- a collection of concepts and tools
- works with Docker containers
- a free open-source project

So, Kubernetes is like Docker-Compose for *multiple machines*

## K8S Architecture & Core Concept

Collection of concepts an tools which can help us deploying containers to anywhere.

![[Pasted image 20240112202937.png]]


![[Pasted image 20240112210207.png]]

![[Pasted image 20240112210233.png]]

## Your Work / Kubernetes' Work

### What Kubernetes Will Do
- Create your objects (e.g. Pods) and manage them
- Monitor Pods and re-create them, scale Pods etc.
- Kubernetes utilizes the provided (cloud) resources to apply your configuration / goals

### What You Need To Do / Setup
- Create the Cluster and the Node Instances (Worker + Master Nodes)
- Setup API Server, Kubelet and other Kubernetes services / software on Nodes
- Create other (cloud) provider resources that might be needed (e.g. Load Balancer, Filesystems)

## Core Components
### Cluster
A set of Node machines which are running the Containerized Application (Worker Nodes) or control other Nodes (Master Node).
### Nodes
Physical or virtual machine with a certain hardware capacity which hosts one or multiple Pods and communicates with the Cluster.
- `Master Node`: Cluster Control Plane, managing the Pods across Worker Nodes
- `Worker Node`: Hosts Pods, running App Containers (+ resources)
### Pods
Pods hold the actual running App Container + their required resources (e.g. volumes).
### Containers
Normal (Docker) Containers
### Services
A logical set (group) of Pods with a unique, Pod- and Container- independent IP address.