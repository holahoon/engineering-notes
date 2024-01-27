In Docker, the term "root" typically refers to the root user within a container. When you run a Docker container, it initially runs as the root user by default. This root user has elevated privileges within the container, similar to the root user on a traditional operating system.
The root user inside a Docker container is separate from the root user on the host system.

It is considered a best practice to avoid running containers as the root user for **security reasons**. Instead, it's recommended to use non-root users whenever possible to reduce the potential impact of security vulnerabilities.

Here's a quick demo on how to change the root user by creating user and group:
```dockerfile
FROM node:21-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

WORKDIR /app

COPY package*.json ./

USER root

RUN chown -R appuser:appgroup .

USER appuser

RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev"]
```

- `FROM node:21-alpine`: Set the base image for your Docker image.
- `RUN addgroup -S appgroup && adduser -S appuser -G appgroup`:
	- `RUN` command creates a group named `'appgroup'` using `addgroup -S` (the `-S` flag indicates that the group should be created system-wide).
	- `adduser -S -G appuser addgroup`: This creates a system user named `appuser` with the `-S` flag indicating it's a system user. The `-G` flag specifies the initial login group for the user, and in this case, it's set to `appgroup`. So, the user `appuser` is added to the existing group `appgroup`.
- `USER appuser`: This sets the default user for subsequent instructions to `appuser`. All following commands in the Dockerfile will be executed with the permissions of the `appuser`.
- `WORKDIR /app`: This sets the working directory for the subsequent instructions to `/app`. Any relative paths in the following instructions will interpreted relative to this directory.
- `COPY package*.json ./`: This copies the `package.json` and `package-lock.json` files from the build context (usually the same directory as the Dockerfile) to the `/app` directory within the container.
- `USER root`: This switches the user back to the root user. Sometimes the ownership of the files in the working directory is changed to root and thus the app can't access the files and throws an error -> `EACCESS: permission denied`. To avoid this, change the ownership of the files to the root user. It's worth nothing that the `USER` instruction persists for the rest of the Dockerfile, so the final `USER root` is setting the default user for any subsequent instructions that might require elevated privileges.
- `RUN chown -R appuser:appgroup .`: This recursively changes the ownership of the current directory(`.`) and all its contents to the user `appuser` and the group `appgroup`. (`chown -R <user>:<group> <directory>`).
- `USER appuser`:  This sets the default user for subsequent instructions back to `appuser`. Any following commands in the Dockerfile will be executed with the permissions of the `appuser`.

The flags for `adduser`:
```
Usage: adduser [OPTIONS] USER [GROUP]

Create new user, or add USER to GROUP

        -h DIR          Home directory
        -g GECOS        GECOS field
        -s SHELL        Login shell
        -G GRP          Group
        -S              Create a system user
        -D              Don't assign a password
        -H              Don't create home directory
        -u UID          User id
        -k SKEL         Skeleton directory (/etc/skel)
```

Now, what if someone sets up user and group from their local host machine using the `-u` and `--group-add` flag with the `docker run` command:
```bash
$ docker run -u appuser:appgroup my-image
```
They can have access to our container file system. Well, this defeats the whole vulnerability reason of setting the user and group right?

We should definitely consider using **Environment Variables** 💡


### Here's a full step-by-step example:
```dockerfile
# set the base image to create the image for react app
FROM node:21-alpine

# create a user with permissions to run the app
# -S: create a system user
# -G: add the user to a group
# this is done to avoid running the app as root
# if the app is run as root, any vulnerability in the app can be exploited to gain access to the host system
# it's a good practice to run the app as a non-root user
RUN addgroup -S appgroup && adduser -S -G appuser appgroup

# set the user to run the app
USER appuser

# set the working directory to /app
WORKDIR /app

# copy package.json and package-lock.json to the working directory.
# this is done before copying the rest of the files to take advantage of Docker's cache.
# if the package.json and package-lock.json files haven't changed, Docker will use the cached dependencies.
COPY package*.json ./

# sometimes the ownership of the files in the working directory is changed to root
# and thus the app can't access the files and throws an error -> EACCESS: permission denied.
# to avoid this, change the ownership of the files to the root user
USER root

# change the ownership of the /app directory to the app user
# chown -R <user>:<group> <directory>
# chown command changes the user and/or group ownership of for given file.
RUN chown -R appuser:appgroup .

# change the user back to the app user
USER appuser

# install dpendencies
RUN npm install

# copy the rest of the files to the working directory
COPY . .

# expose port 5173 to tell DOcker that the container listens on the specified network ports at runtime
EXPOSE 5173

# command to run the app
CMD ["npm", "run", "dev"]
```


