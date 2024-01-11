
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

Take a look at this issue: [[amd64 and arm64 issue]]

The local built image must be the same name as the one on Docker Hub(i.e. `holahoon/udemy-docker-example`). If the names are different, we can always change the name:
```bash
$ docker tag <local-image-name> <docker-hub-image-name>
```

Then we can push the image to Docker Hub (i.e. `holahoon/udemy-docker-example`):
```bash
 $ docker push holahoon/udemy-docker-example
```



