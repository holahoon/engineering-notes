## Stop & Restart a container

Stop a container by:
```bash
$ docker stop <container-name>
```

Restart a container by:
```bash
$ docker start <container-name>
```

If I restart the container, notice how the process in the terminal "stops running" even though I can see that the container is running in the background by:
```bash
$ docker ps
```

## Understand Attached & Detached Container

Notice how when running `docker run -p <port> <image-id>`, it "blocks" any commands in the terminal, but running `docker start <container-name>` doesn't.
This is because by default, run is an attached mode and start is a detached mode.
The "attached" mode simply means that I am listening to the output of that container. For instance, if I have a `console.log`, I can see the output in my terminal since the container is attached, but I won't be able to see the output when the container is detached.

### Detach a container initially by:
```bash
$ docker run -p <port> -d <image-id>
```

### Attach myself to a detached container by:
```bash
$ docker container attach <container-name>
```

### Attach myself to a container when restarting by:
```bash
$ docker start -a <container-name>
```

### Print the logs from the container by:
```bash
$ docker logs <container-name>
```
Or, add `-f` to keep on listening to the logs.



