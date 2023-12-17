Docker supports build-time ARGuments and runtime ENVironment variables.
Build ARGuments and Runtime ENVironment variables can be used to make images and containers more **dynamic** / **configurable**.

### ARG
- Available inside of `Dockerfile`, NOT accessible in CMD or any application code.
- Set on image build (`docker build`) via `--build-arg`.

### ENV
- Available inside of `Dockerfile` & in application code.
- Set via ENV in `Dockerfile` or via `--env` or `docker run`.

## Working with Environment variables

We can set environment variables in the docker file as well:
```dockerfile
...
COPY . .

ENV PORT 80 # <--

EXPOSE $PORT # <--

CMD [ "npm", "start" ]
```
The `$` sign indicates that it is an env variable we set in our docker file. `ENV PORT` is the name of the environment, and `80` is the value of it (this is the default value).

Or we can configure within our terminal when running the container:
```bash
$ docker run ... -p 3000:8000 --env "PORT=8000" ...
```
We need to expose our PORT env variable that we just assigned: `3000:8000`. We can also just use `-e`.
Btw, we can also have multiple env variables as well.

We can also create a `.env` file in our root directory and store `PORT=8000` in there. Now in order to run this file:
```bash
$ docker run ... -p 3000:8000 --env-file ./.env ...
```

## Working with ARG variables

Just like environment variables, you can set `ARG` variables inside the docker file as well:
```dockerfile
...
RUN npm install

COPY . .

ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT # <--

EXPOSE $PORT
...
```

*Tip: try to place the `ARG` where it needs. Remember that each command is a layer in an image, so if we change the `DEFAULT_PORT` when building an image, it could run `npm install` again if `ARG` was placed above `RUN`*.

This would set the default `ARG` in build time.

We can also override our default arg when building an image with `--build-arg <NAME=VALUE>`:
```bash
$ docker build -t feedback-app:dev --build-arg DEFAULT_PORT=8000
```

