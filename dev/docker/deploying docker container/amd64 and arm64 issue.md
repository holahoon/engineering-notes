🚩 One thing I noticed: I'm currently using Mac M2. This uses linux/arm64 platform. Whenever I try to run the image on AWS EC2 instance, it complains `WARNING: The requested image's platform (linux/arm64/v8) does not match the detected host platform (linux/amd64/v3) and no specific platform was requested`.
I tried adding `--platform linux/amd64` when running the docker image on EC2 instance, it would NOT solve...

If I just push, it uses the `linux/arm64` with Mac M2 chip.
![[Pasted image 20240110235743.png]]

My workaround was to just push the built image with `linux/amd64` platform by adding `--platform=linux/amd64` in the build command:
```bash
$ docker build --platform=linux/amd64 -t <image-name> .
```
![[Pasted image 20240110235855.png]]
I created a new repository, and notice how it uses `linux/amd64`.

### Using docker file

We can simply just add a command in our docker file:
```dockerfile
FROM --platform=linux/amd64 node:14-alpine
```
This will tell Docker to use `linux/amd64`.