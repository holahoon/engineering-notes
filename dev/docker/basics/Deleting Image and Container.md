There are times when I need/want to delete containers or images.

## Delete Containers

I can simply run:
```bash
$ docker rm <container-name>
```
This removes any "stopped" container.
I can also remove multiple containers by separating the containers' names with a white space.

## Delete Images

I can first check to see any images in my local machine:
```bash
$ docker images
```

I'll probably get an output like this if there are any:
![[Pasted image 20231211185747.png]]

In order to delete an image:
```bash
$ docker rmi <iamge-id>
```
![[Pasted image 20231211185902.png]]

Notice how it deletes all the layers inside the image. I can also delete multiple images with white spaces between them.

Keep in mind that any images that belong to a container whether the container is running or stopped, CANNOT be removed. The container needs to removed first.

I can also remove all the images that are not being used in the running containers atm:
```bash
$ docker image prune
```

## Automatically Remove a Container

I can also remove a container when it exits:
```bash
$ docker run -p <port> --rm <image-id>
```

If I stop running this container with and check the list:
```bash
$ docker stop <container-name>
$ docker ps -a
```
The container will not be on the list(it should be on the list if not removed).



