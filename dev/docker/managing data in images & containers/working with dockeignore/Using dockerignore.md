Similar concept to `.gitignore` where we want to ignore certain files in building docker image. We normally include folders/files such as `node_modules` which prevents from committing through git.

We can just create a `.dockerignore` file and also include `node_modules` folder. This is because by including `node_modules`(it would exist if we run `npm install` in our local environment), it would override the `node_modules` folder created via `npm install` inside of the Image.
Our current existing `node_modules` could be out-dated or missing important dependencies, or even if it is up-to-date, it simply makes the `COPY` process take a bit longer.

## Other Files to ignore
### Log Files
Log files can be large and not really necessary in a Docker image, so: `*.log`.

### Git Files
It's also a good idea to ignore `.git` and `.gitignore` files.

### Environment Files
It's a good idea to exclude some `.env` files that store sensitive information to maintain the security of your application.

[Reference](https://hn.mrugesh.dev/how-to-use-a-dockerignore-file-a-comprehensive-guide-with-examples)

