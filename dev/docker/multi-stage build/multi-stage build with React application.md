Here's a multi-stage build using React application.

Here's a simple development docker file:
`Dockerfile`
```dockerfile
FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "npm", "start" ]
```

In development, we would run the `npm start` command to start our development environment in our host machine(docker in this example exposing port `3000`).

It is different in deployment for production though:
`Dockerfile.prod`
```dockerfile
FROM node:14-alpine as fe-build

WORKDIR /app

COPY package.json .  

RUN npm install

COPY . .

RUN npm run build

# switch to a different based image
FROM nginx:stable-alpine

# copy the final content from the fe-build stage before switching
COPY --from=fe-build /app/build /usr/share/nginx/html

# nginx default port
EXPOSE 80

# start nginx server
CMD ["nginx", "-g", "daemon off;"]
```
It first builds the frontend application(React in this case), and based on the build(`fe-build`), it uses `nginx` to start the server.

## Building Docker Image

Let's take a look at how to build a monorepo:
Say we have the following project structure:
```
├── backend
├── env
├── frontend
│   ├── src
│   ├── Dockerfile
│   ├── Dockerfile.prod     # for production build
│   └── ...
├── docker-compose.yaml
└── Dockerfile
```

Here's just an example of the `docker-compose` file:
`docker-compose.yaml`
```yaml
version: '3.8'
	services:
		backend:
			build: ./backend
			ports:
				- '80:80'
			volumes:
				- ./backend:/app
				- /app/node_modules
			env_file:
				- ./env/backend.env
		frontend:
			build: ./frontend
			ports:
				- '3000:3000'
			volumes:
				- ./frontend/src:/app/src
			stdin_open: true
			tty: true
			depends_on:
				- backend
```

We can build an image:
```bash
$ docker build -f frontend/Dockerfile.prod -t <image-name> ./frontend
```
Here, I'm specifying to use the docker file located in `./frontend/Dockerfile.prod` path to build an image based on the command. `-f` option allows to specify the docker file name.

### Understanding `--target` in multi-staged builds

Here's a really cool concept of using target.
By looking at the `Dockerfile.prod` from above, I gave a name of `fe-build` for the `node` build stage.
We can add `--target` to let Docker know which build stage to only use (in our case, the `fe-build`).
```bash
$ docker build --target fe-build -f frontend/Dockerfile.prod ./frontend
```

