
Here's an example of a Node backend that stores a txt file inside `/story/text.txt`
`app.js`
```javascript
const path = require('path');
const fs = require('fs');

const express = require('express');
const bodyParser = require('body-parser');

const app = express();

const filePath = path.join(__dirname, 'story', 'text.txt');

app.use(bodyParser.json());

app.get('/story', (req, res) => {
	fs.readFile(filePath, (err, data) => {
		if (err) {
			return res.status(500).json({ message: 'Failed to open file.' });
		}
		res.status(200).json({ story: data.toString() });
	});
});

app.post('/story', (req, res) => {
	const newText = req.body.text;
	
	if (newText.trim().length === 0) {
		return res.status(422).json({ message: 'Text must not be empty!' });
	}
	
	fs.appendFile(filePath, newText + '\n', (err) => {
		if (err) {
			return res.status(500).json({ message: 'Storing the text failed.' });
		}
		res.status(201).json({ message: 'Text was stored!' });
	});
});

app.get("/error", () => {
	process.exit(1);
});

app.listen(3000);
```

### Create Docker files
we have a docker file:
`Dockerfile`
```dockerfile
FROM --platform=linux/amd64 node:14-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .  

EXPOSE 3000

CMD [ "node", "app.js" ]z
```
side note: I just wanted to use the `linux/amd64` platform since it throws an error in my Mac M2 chip.

Then we have a docker-compose yaml file (just to try to run it locally).
`docker-compose.yaml`
```yaml
version: "3"
services:
	stories:
		build:
			context: .
			dockerfile: Dockerfile
		volumes:
			- stories:/app/story
		ports:
			- 80:3000
volumes:
	stories:
```

Let's try to build an image and push to Docker Hub (_make sure to create a repository in Docker Hub first though_):
```bash
$ docker build -t holahoon/k8s-story .
$ docker push holahoon/k8s-story
```

### Create k8s files

`deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: story-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: story
	template:
		metadata:
			labels:
				app: story
		spec:
			containers:
				- name: story
				  image: holahoon/k8s-story
```

`service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
	name: story-service
spec:
	selector:
		app: story
	type: LoadBalancer
	ports:
		- protocol: "TCP"
		  port: 80
		  targetPort: 3000
```

Make sure that minikube is up and running (docker in my case):
```bash
# start the driver
$ minkube start --drive=docker

# check status
$ minkube status
```

Since we have the k8s yaml files, we can apply these commands:
```bash
$ minkube apply -f=deployment.yaml -f=service.yaml
```

And now, let's look at the running Service:
```bash
$ minikube service story-service
```
This will open up a URL and we can test the endpoints with Postman or any preferred API tools. 🎉

## Issue 💥

There's a problem with this. The volumes created will be LOST when the Pods containing the container is restarted or crashes due to say... high traffic.