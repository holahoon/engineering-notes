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

VOLUME ["/app/feedback"]

CMD ["node", "server.js"]
```
`/app/feedback` is the path that I want to map to the folder outside of the container. So any file that being created/modified/removed within the container's  `/app/feedback` folder should reflect to my host machine.