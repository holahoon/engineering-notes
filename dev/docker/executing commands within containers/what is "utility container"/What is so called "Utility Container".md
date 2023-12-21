So called "utility containers" allow you to create a container and initialize a project inside the container. You typically would initialize in your local machine and containerize it, but instead, you can create a container and initialize it inside the container and reflect it in the local machine as well (via bind mounts).

![[Pasted image 20231220195153.png]]

Say I want to start a Node application, I would need to install node in my machine and run `npm init` to initialize my node application. Here's some cool ways of doing it with _utility containers_.

## Using Docker Commands

### Let's first build an image with [Node](https://hub.docker.com/_/node).

I'm just going to give it a tag of `util-node`
```bash
$ docker build -t util-node .
```
This command will create a Node image.

### Run the Container

Here's some cool commands to use to run and reflect in our local machine.
```bash
$ docker run -it -v $(pwd):/app util-node npm init
```
We need to run the container in _interactive mode_ and bind mount to have our local machine to have what's on the container and vice versa.
We can also run some commands after the image name(`util-node`).

Another cool feature to know is the usage of `ENTRYPOINT`.
```dockerfile
FROM node:14-alpine

WORKDIR /app

# whatever the command is after the image name on docker gets appended
ENTRYPOINT [ "npm" ]
```
With `ENTRYPOINT`, we can skip the need of having to type in `npm`. Docker recognizes the entry point and uses it:
```bash
$ docker run -it -v $(pwd):/app util-node install express --save
```
We can even install some packages too!

## Using Docker Compose
It's pretty cumbersome to write all these long commands...

`Dockerfile`
```dockerfile
FROM node:14-alpine

WORKDIR /app

# whatever the command is after the image name on docker gets appended
ENTRYPOINT [ "npm" ]
```
Remember that we are still using `ENTRYPOINT` of `npm`.

`docker-compose.yaml`
```yaml
version: "3.8"
	services:
		npm-cmd:
			# [ where dockef file is located ]
			build: ./

			# [ -it flag ]			
			stdin_open: true
			tty: true
			
			# [ bind mount ]
			volumes:
				- ./:/app
```

`bash`
```bash
$ docker-compose run --rm npm-cmd init
```
`docker-compose run` allows us to run a single service by the service name (`npm-cmd`) with commands to append after the entry point of our choice (`init`).

`docker-compose up` and then `down`, the containers are automatically removed. `run` does not automatically remove containers. So just using `--rm` flag can help to clean up stopped containers.
