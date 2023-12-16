Volumes are managed by Docker.

Volumes are folders on your host machine hard drive which are mounted ("_made available_" or _mapped_) into containers.
So basically, volumes are folders in my computer, which you make docker aware of, and which are then mapped folders inside of a  docker container.
It's pretty much a connection a folder inside of a container with a folder outside of a container(on my host machine). This allows any changes in either of the folder will be reflected on the other one.

Volumes **persist even if a container shuts down**. If a container (re-)starts and mounts a volume, any data inside of that volume is **available in the container**.
A container **can write** data into a volume and **read data** from it.

## How to add a volume

Here's a small example
```dockerfile
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# This creates an anonymous volume.
VOLUME ["/app/feedback"]

CMD ["node", "server.js"]
```
`/app/feedback` is the path that I want to map to the folder outside of the container. So any file that being created/modified/removed within the container's  `/app/feedback` folder should reflect to my host machine.

## Two Types of External Data Storages

### Volumes

- Anonymous Volumes
- Named Volumes

### Anonymous Volumes
- Created specifically for a single container.
- Survives container shutdown / restart unless `--rm` is used.
- Cannot be shared across containers.
- Since it's anonymous, it can't be re-used (even on same image).
- Useful for "locking in" certain data which already exists in the container.
- Useful for when avoiding certain data being overwritten by another module.
### Named Volumes
- Created in general - not tied by any specific container.
- Survives container shutdown / restart -- removal via Docker CLI.
- Can be shared across containers.
- Can be re-used for same container (across restarts).

Are great for data which should be persistent but which you don't need to edit directly(volumes in your host machine are hidden somewhere and it is not meant to be found). Named volumes are NOT closely attached to one specific container, it still lives even if a container shuts down.

Docker sets up a folder / path on your host machine, exact location is unknown to you (=dev). Managed via _docker volume_ commands. You can see the help list with `docker volume --help`. You can also see the list of volumes with `docker volume --list`.

A defined path in the container is mapped to the created volume / mount. e.g. `/some-path` on your host machine is mapped to `/app/data`.

By adding `VOLUME ["/app/feedback"]` like the above examples creates an anonymous volume, so in order to create a named volume, get rid of the `VOLUME` line from your docker file,:
```bash
# Let's just create an image with repository named `feedback-node` and give it a tag `volumes`
$ docker build -t feedback-node:volumes .

# Assign a name to a volume when running a container
# -p 3000:80: Local port 3000 with port 80 server
# -d: detached mode
# --rm: remove the container after stopping
# --name: name the container as `feedback-app`
$ docker run -p 3000:80 -d --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes
```
- `-v`: volumes.
- `feedback:`: Name chosen to store the volumes.
- `/app/feedback`: When docker creates volumes, this folder / path is connected to the container.
You can try `docker volume ls` and see that the volume has a name and even if the `feedback-app` stops running, the volume persists.
![[Pasted image 20231215142841.png]]

## Removing Volumes

Anonymous volumes are not connected to newly created containers, sometimes the anonymous volumes can pile up. Then you can just run:
```bash
$ docker volume rm <VOL_NAME>

# or remove all volumes
$ docker volume prune
```


## [[Bind mounts]]
