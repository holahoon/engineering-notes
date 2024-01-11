
Login to AWS and create an EC2 instance. Make sure to create a secret key(`.pem`) and store it locally (to NOT share it).

Use SSH client to connect to the instance.
![[Screenshot 2024-01-10 at 7.45.57 PM.png]]
Running the `ssh -i ...` will connect a remote SSH terminal.
Follow the steps and run the commands in the connected remote SSH terminal:

```bash
$ sudo yum update -y
$ sudo yum -y install docker
$ sudo service docker start
$ sudo usermod -a -G docker ec2-user
```
Make sure to log out and log back in after running the last command.
Once logged back in:
```bash
$ sudo systemctl enable docker

# check if docker is usable
$ docker version

# try to run docker container -- which will complain, because no commands were given
$ docker run
```

## Deploying Built Image vs Source Code

We have two options to deploy.
1. Deploy source
2. Deploy built image

We would need to build an image on remote machine, push source code to remote machine via: `run docker build` and then `docker run`. This adds unnecessary complexity.
Instead, with option 2, we can just build an image before deployment(e.g. on local machine). Then, just execute `docker run`. This will avoid unnecessary remote server work.

## Create an Image on Docker Hub

Let's try to create an image from the host machine and push it to Docker Hub.
We obviously need a docker file:
```dockerfile
# Just an example
FROM node:14-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "app.js"]
```
Go to Docker Hub and create a repository.
Open up a new terminal(do NOT use the AWS terminal).
Make sure to add a `.dockerignore` file and add files or folders that needs to be ignored (especially `*.pem` where it has our secret key code).

Let's start building an image:
```bash
$ docker build -t <image-name> .
```

🤔 Wait! have a take a look at this issue: [[amd64 and arm64 issue]]

The local built image must be the same name as the one on Docker Hub(i.e. `holahoon/udemy-docker-example1`). If the names are different, we can always change the name:
```bash
$ docker tag <local-image-name> <docker-hub-image-name>
```

Then we can push the image to Docker Hub (i.e. `holahoon/udemy-docker-example1`):
```bash
 $ docker push holahoon/udemy-docker-example
```

We can now run an image that we pushed to Docker Hub:
```bash
# SSH terminal
$ docker run -d --rm -p 80:80 holahoon/udemy-docker-example1
```
This runs an `holahoon/udemy-docker-example1` image exposing port 80!.

## Configure Security Group to expose all required ports to WWW

We need to configure on AWS EC2 the security protocols.
Go to the running EC2 instance and take a look at Security tab:
![[Pasted image 20240111114126.png]]
![[Pasted image 20240111114153.png]]
This shows that we are using the `launch-wizrd-3`. So if we go in there, we need to set the inbound rules that exposes the HTTP connection (type: HTTP) from anywhere in the world (Anywhere-IPv4).
![[Pasted image 20240111114230.png]]
This will allow us to visit the public IPv4 and we can see our server up and running.
![[Pasted image 20240111114357.png]]

Up to here, we have successfully pushed our built docker image to Docker Hub, have the container running in our AWS EC2 instance. With docker, we didn't have to install Node environment in EC2 instance 👏.


