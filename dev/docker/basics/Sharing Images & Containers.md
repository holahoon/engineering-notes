Everyone who has an image, can create containers based on the image!

## Share a Dockerfile

I can simply run:
```bash
$ docker build .
```
This will build a container based on the received `Dockerfile`.
Keep in mind that the `Dockerfile` instructions might need surrounding files / folder (e.g. source code).

## Share a Built Image

I can download an image, run a container based on it!
There's no build step required, everything is included in the image already.

## Sharing via Docker Hub or Private Registry

### Docker Hub
- Free Usage Possible
- Official Docker image registry
- Public, private and "official" images

### Private Registry
- Any provider / registry I want to use
- Only my own (or team) images

## Pushing an Image to Docker Hub

It's really easy to set up an account with Docker Hub.
Create a repository, for my case a sample name of `node-hello-world-dk`. This would create a repository named `holahoon/node-hello-world-dk` which also includes my user name.

Now, in order for me to push to the hub, I can run the following command:
```bash
$ docker push holahoon/node-hello-world-dk
```
(I can also include a `tagname` shown in the General page on Docker Hub).

BUT!, I need to set the name in my local environment before pushing it

If I have a different name, I would either have to build a new one or rename an existing one with the image name `holahoon/node-hello-world-dk`.

I can simply just build a new image:
```bash
$ docker build .
```

Or just rename the existing image:
```bash
$ docker tag <old-image-name> holahoon/node-hello-world-dk
```
This just renames the image and I can now run `docker push`

## Pulling an Image

Simply run:
```bash
$ docker pull <repository-name>
```
In my case, the repository name will be `holahoon/node-hello-world-dk`.
Then, I can simply run a container in my local environment.

Keep in mind that docker will always pull the latest image of the repository name from the container registry.
Also, by running `docker run <repository-name>` in my local env will try to find if there is any image in my local env, and if it doesn't find it, it will look for the image in the Docker Hub.
But, it's just like Git. I will NOT have the latest whenever I run the `run` command thinking that it will pull from the hub. If someone else pushes a new image with the latest tag, I would also need to pull that image as well.

