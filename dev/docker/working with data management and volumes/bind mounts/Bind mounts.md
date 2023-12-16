Managed by you (myself)

- Location on host file system, not tied to any specific container.
- Survives container shutdown / restart - removal on host fs.
- Can be shared across containers.
- Can be re-used for same container (across restarts).

Whenever an image is built and container is ran, when changing code in files in my local environment will NOT automatically get reflected to the running container. It would require to rebuild an entire image and run containers.
This is pretty cumbersome.

Binding mounts are great for persistent, editable(by you) data (e.g. source code).

Bind mount is specific to a running container, not to the image. It only affects the container. So, you need to setup a bind mount from inside the terminal when running a container.

Here's a sample docker file:
```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD ["node", "server.js"]
```

```bash
# Build an image
$ docker build -t feedback-node:volume .

# volume name: feedback
# volume path: /app/feedback
$ docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v "/Users/dk/Desktop/code/udemy/docker/4_data-volumes-01-starting-setup:/app" feedback-node:volumes
```

The path of which the folder you want the volume to bind must be an absolute path. You can simply copy/paste it from VSCode editor right clicking and `copy path`. Here, I want the whole folder to be bound to the volume.
They reason for double quotes is that there could be spaces in some folders and could potentially lead to an unexpected error(?). 
`:/app` is used because you want to copy all the source codes inside `/app`. Look at `WORKDIR /app`.

TIP: There's a shortcut for absolute paths:
- macOS / Linux: `-v $(pwd):/app`
- Windows: `-v "%cd%":/app`

Make sure Docker has access to the folder as well.
![[Pasted image 20231215155146.png]]

Up to this point, there's a little problem. Since we are copying the local folder to the running container, it will not install any dependencies that our project relies on. If running a Node server, it could throw an error saying "Cannot find module 'express". This is because it copies all the folders to the container and overrides, but never installs it. So could be missing folders/files such as `node_modules`.
To resolve this, we need an [[Understanding Volumes#Anonymous Volumes]]. 
We can simply add `VOLUME ["/app/node_modules"]` to the docker file, or we can just add a command when running the container:
```bash
$ docker run -p 3000:80 ... --name feedback-app -v feedback:/app/feedback -v $(pwd):/app -v /app/node_modules feedback-node:volumes
```
(_Keep in mind that `-v feedback:/app/feedback` is creating a named volume. Has nothing to do with binding mounts_).
Here, the `-v /app/node_modules` creates an anonymous volume.

Docker always evaluates all the volumes set on the container, and there's any crashes, the _longer internal path_ wins. So here, docker sees that `$(pwd):/app` than `/app/node_modules` folder is more specific and it will override any same files/folders. Hence, since `$(pwd):/app` does not have `node_modules`, so `/app/node_modules` will override a non-existing `node_modules` folder from the `$(pwd):/app`.

TLDR; anonymous volume prioritizes internal path over external path.

## Using `nodemon`

This is something that's pretty cool. Since It is Node that runs our JavaScript files, if any changes to the JavaScript file and trying to see some logs (e.g, `console.log`), we would need to re-run the container again.
Here's using `nodemon`.
### Install `nodemon` and set scripts
Install `nodemon` and create a command in `package.json`:
```json
...
"scripts": {
	"start": "nodemon server.js"
},
"dependencies": {
	"body-parser": "^1.19.0",
	"express": "^4.17.1"
},
"devDependencies": {
	"nodemon": "2.0.4"
}
```
### Update docker file
Write a command script to let docker know:
```Dockerfile
...
CMD ["npm", "start"]
```
### Rebuild the image and run a container
```bash
# Build docker image
$ docker -t feedback-node:volumes .

# Run docker container
$ docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback -v $(pwd):/app -v /app/node_modules feedback-node:volumes

# Look at Docker logs
$ docker logs feedback-app
```
Now, you'll be able to see either your `console.log` or `nodemon` messages.
