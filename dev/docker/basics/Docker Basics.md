## Containers
Small packages that contain both the application/website/server and the entire environment to run that application. It's the running "unit of software".

## Images
Template/blueprints for the containers. It's the "image" that contains the code and the required tools to execute the code.

This image can have the NodeJS environment or the NodeJS application code. The container has the running NodeJS application.

To summarize, images behind the scenes, hold all the logic and all the code the container needs, and then we create instances of an image with the `run` command. This creates a concrete container based on an image.

## Some examples


```bash
$ docker build .
```
builds the docker container. With the built container ID

```bash
$ docker run -p 3000:3000 <container-id>
```
will spin up a container with the image inside port 3000

![[Pasted image 20231210145207.png]]

We can stop the running docker server with
```bash
$ docker stop <NAMES>
```

We can check the current status of the containers
```bash
$ docker ps
```
This command will give us the running containers.

We can also check all the lists by adding `-a` (short form of the `--all`). This command shows both the stopped and running containers.
```bash
$ docker ps -a
```

We can also run the following command to containerize a Node environment.
```bash
$ docker run -it node
```
This creates a container of a Node environment and can start playing around with it =) really cool!

## Building an Image
[[Docker file to build an image]]

## Managing Containers
[[Managing Containers]]

## Interactive Mode
[[Interactive Mode]]

## Deleting Image & Container
[[Deleting Image and Container]]

## Inspecting Images
[[Inspecting Images]]

## Copy from/to a Container
[[Copy from or to a Container]]

## Assigning Names & Tags to a Container
[[Assign Names and Tags to a Container]]

## Sharing Images & Containers
[[Sharing Images & Containers]]


## Conclusion
Docker is all about Images & Containers
- Images are the templates / blueprints for Containers, multiple Containers can be created based on one Image.
- Images are either downloaded (`docker pull`) or created with a `Dockerfile` and `docker build`.
- Images contain multiple layers (1 instruction = 1 layer) to optimize build speed (caching!) and re-usability.
- Containers are created with `docker run <image>` and can be configured with various options / flags
- Containers can be listed (`docker ps`), removed (`docker rm`) and stopped + started (`docker stop / start`).
- Images can also be listed (`docker images`), removed (`docker rmi`, `docker image prune`) and shared (`docker push / pull`).
