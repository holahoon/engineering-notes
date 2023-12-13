```dockerfile
FROM node

WORKDIR /app

COPY . /app

RUN npm install

EXPOSE 80

CMD ["node", "server.js"]
```

- `FROM`: from where I want to pull an image from. This would be the base image.
- `COPY`: which files that live in my local machine should go in the image.
	- _first dot_: path outside of the container/image where the files live that should be copied into the image (excluding the docker file). So for the example case, every files in the current root dir will be copied (by having the docker file in the same directory).
	- _second path_: path inside the image to store the files in. All the files copied from the "first dot' will be created in the `app` folder.
- `WORKDIR`: Any subsequent commands (i.e. `npm install`) will be executed from inside that folder. Basically, run `npm install` in `/app` within our docker container.
- `RUN`: The command to run. This will be executed when an image is created.
- `EXPOSE`: When the container starts, expose a certain port to our local system. With this, I will be able to run the container that listens on that port.
- `CMD`: I don't want to "start a server" when creating an image, I want to start it by creating a container inside an image. This command will execute when a container is started based on an image.

## Build an image

In order to build an image,
```bash
$ docker build .
```
I can run this command to first build an image. Since the docker file is within the root directory, I can just specify the current dir with `.`.

### Run a container

Run the following commands to run the created container
```bash
$ docker run -p 3000:80 <container-id>
```
This command runs the container with a local port `3000`, and then with the internal docker container expose port: `80`.

## Image layer

All the commands line by line are layers. If there is no changes to each layers, it will just use the cache which can improve a significant performance. 

Say if I change a line of code that lives within `/src/app.js`. I would need to re-build the docker image again. So up to `WORKDIR /app` command, it will use the cached results and starting with `COPY . /app`, it would need to start a new build from this layer. Docker will re-build any layers after this layer. This means that in our case, I would need to run `npm install` again even though nothing has changed in our `package.json` file. This may not be a problem, but if a project has a lot of node dependencies, it would take some time which is unnecessary.

One improve I can make is to restructure our docker file in a way that we can keep `package.json` in cache, and move the COPY command:
```dockerfile
FROM node

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node", "server.js"]
```
If I build the image again, it should only grab the cached result from the code changes. It will not `npm install` again since there was no change in the `package.json` file.

Boom! our build took like 0.0s literally.



