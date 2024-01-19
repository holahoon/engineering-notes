It automates multi-container setup. It allows to use all these `docker build`, `docker run` commands.
Just with one configuration file and orchestrate commands.
Docker compose creates a default network and add all the containers to that network.
## What Docker Compose is NOT
- Docker Compose does **NOT replace** `Dockerfiles` for custom images.
- Docker Compose does **NOT replace images** or **containers**.
- Docker Compose is **NOT suited** for managing multiple containers on different hosts (machines)

## Writing Docker Compose Files
Here's an example of using docker compose file

I have a project with backend and frontend along with a database:
```md
├── frontend(react)
│   ├── src
│   ├── Dockerfile
├── backend(node)
│   ├── app.js
│   ├── Dockerfile
│   ├── env
│	│   ├── backend.env
│	│   ├── mongo.env
└── docker-compose.yaml
```

```yaml
docker-compose.yaml
# The version I want to use
version: "3.8"

services:
	# docker automatically picks up the name of the container(mongodb) to be used in mongodb connection url
	mongodb:
		image: "mongo"
		
		volumes:
			- data:/data/db

		# environment:
			# # - MONGO_INITDB_ROOT_USERNAME=dk
			# MONGO_INITDB_ROOT_USERNAME: dk
			# MONGO_INITDB_ROOT_PASSWORD: secret
		env_file:
			- ./env/mongo.env

		# (optional) docker already creates a network for all the containers specificed
		# networks:
			# - goals-net

	backend:
	
		# where to find the Dockerfile
		build: ./backend
		# build:
			# context: ./backend
			# dockerfile: Dockerfile
			# args:
				# some-arg: 1

		# host port : internal port
		ports:
			- "80:80"

		volumes:
			- logs:/be/logs		
			# bind mount can be a relative path in docker-compose instead of absolute path
			- ./backend:/be

			# anonymous volumes
			- /app/node_modules
		
		env_file:
			- ./env/backend.env

		# it should first bring up the dependent service (mongodb)
		depends_on:
			- mongodb

  

	frontend:
	
		build: ./frontend
	
		ports:	
			- "3000:3000"
		
		volumes:
			# bind mount
			- ./frontend/src:/fe/src
		
		# -it flag
		stdin_open: true
		tty: true
		
		depends_on:
			- backend

# all named volumes must be listed (not bind mounts or anonymous volumes)
volumes:
	data:
	logs:
```
## Docker Compose Commands

To start docker compose:
```bash
$ docker-compose up
```
This command builds, pulling all the images and start a container.

- add `-d` to run it in detached mode.
- add `--build` to rebuild the image

To stop docker compose and and remove:
```bash
$ docker-compose down
```
When the service is down, it **automatically removes** the container (`--rm`)
This does not delete volumes. In order to remove volumes, add `-v` flag.

