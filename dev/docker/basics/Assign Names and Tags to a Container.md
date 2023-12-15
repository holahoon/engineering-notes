## Assign Name to a Container

It could be a troublesome by having to either memorize or look up created docker container names.

There's a command to assign a name to a container:
```bash
$ docker run -p 3000:80 --name tag-name 0ebf92fe547c
```
I just assigned `tag-name` to the container.

## Assign Tag to an Image

If I just build an image:
```bash
$ docker build .
```
It creates an image with an id of say: `sha256:bd7a0e9a9a3dd5835da99e4f34d55d8dd1bb9f2239f0046fe0141da1a8d3d436`.
It is `name(repository) : tag`.
- `name`: I can define a group specialized images
- `tag`: I can define a specialized image within a group of images.

I can specialize a tag for an image:
```bash
$ docker build -t test-image:latest .
```
Of course, giving the repository a name or a tag is optional.
This will result:
![[Pasted image 20231211213008.png]]

So now, I can run a container with the new tagged container by giving the container a name too:
```bash
$ docker run -p 3000:80 --name whoa test-image:latest
```
This would start a new container named `whoa` within `test-image:latest` image.

## Removing ALL images, including tagged images

To remove all images, including the tagged images:
```bash
$ docker image prune -a
```
