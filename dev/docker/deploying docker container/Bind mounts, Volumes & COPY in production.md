## In Development

- Containers should encapsulate the runtime environment but not necessarily the code
- Use "Bind Mounts" to provide your local host project files to the running container
- Allows for instant updates without restarting the container

## In Production
Image / Container is the "single source of truth"
- A container should really work standalone, you should NOT have source code on your remote machine
- Use `COPY` to copy a code snapshot into the image
- Ensures that every image runs without any extra, surrounding configuration or code

