I can extract from or add to a container:
```bash
$ docker cp
```

- `cp`: copy files or folders into or out of a running container.

So if I have a running container:
```bash
# If no image was built,
$ docker build .

# If no container is running
$ docker run -p 3000:80 <image-id>

# If a container already exists, but want to restart the container by attaching
$ docker start -a <container-name>
```

Say I create a dummy text file inside the root dir:
```markdown
├── src
├── dummy
│   ├── dummy.text
├── public
├── Dockerfile
├── package.json
└── server.js
```

This `dummy/dummy.text` can be copied into an existing running container:
```bash
$ docker cp dummy/. <container-name>:/dummy-copy
```

- `dummy/.`: says I want to copy everything inside the `dummy` folder.
- `<container-name>:/dummy-copy`: says I want to paste it inside the container's `dummy-copy`.

I can do the vice-versa by switching the commands' order to copy into the local environment (Try deleting the `dummy` folder completely):
```bash
$ docker cp <container-name>:/dummy test/.
```
It copies everything inside the `dummy` dir from the container and pastes by creating if no `test` folder exists in my local environment.

## When it is useful

This command can be useful:
- allows to add something to the container without rebuilding an image or restarting a container.
- 

### WARNING!
It might be tempting to just copy the changed codes' files into the container. It is very error-prone. Do NOT do it. It is pretty easy to forget a file or two.
Also, it's not really possible to replace a file that is already executed (like the `server.js` file).

There's a better way of updating the codes without having to rebuild the image.

There are scenarios where copying something into a container. For example, configuration files for a web server.
