Let's say we have a frontend React application that we also want to deploy.
This project continues from [[_example source code]]
#### project structure
```
├── frontend
│	├── conf
│	├── Dockerfile
│	├── public
│	├── ...
│	└── src
├── auth-api
├── users-api
├── tasks-api
├── k8s
├── docker-compose.yaml
└── ...
```
#### frontend app
`fetchTasks()` sends GET request to fetch data (via button `onClick` event). `addTaskHandler()` sends a POST request so that the `tasks-api` backend handler can write to a txt file.
The `http://127.0.0.1:54859` IP is the IP address provided by `tasks-service` (`k8s/tasks-service.yaml`). This can be found when running `minikube service tasks-service` (IP address can be different when starting a new Service).

`frontend/src/app.js`
```javascript
...
const fetchTasks = useCallback(function () {
	fetch("http://127.0.0.1:54859/tasks", {
		headers: {
			Authorization: "Bearer abc",
		},
	})
	.then(function (response) {
		return response.json();
	})
	.then(function (jsonData) {
		setTasks(jsonData.tasks);
	});
}, []);

function addTaskHandler(task) {
	fetch("http://127.0.0.1:54859/tasks", {
		method: "POST",
		headers: {
			"Content-Type": "application/json",
			Authorization: "Bearer abc",
		},
		body: JSON.stringify(task),
	})
	.then(function (response) {
		console.log(response);
		return response.json();
	})
	.then(function (resData) {
		console.log(resData);
	});
}

useEffect(
	function () {
		fetchTasks();
	},
	[fetchTasks]
);

return (
	...
)
```

#### nginx configuration
`frontend/conf/nginx.conf`
```conf
server {
	listen 80;

	location / {
		root /usr/share/nginx/html;
		index index.html index.htm;
		try_files $uri $uri/ /index.html =404;
	}

	include /etc/nginx/extra-conf.d/*.conf;
}
```

#### frontend docker file
`frontend/Dockerfile`
```dockerfile
FROM node:14-alpine as builder

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

RUN npm run build  

FROM nginx:1.19-alpine

COPY --from=builder /app/build /usr/share/nginx/html

COPY conf/nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD [ "nginx", "-g", "daemon off;" ]
```

#### frontend k8s configurations
`k8s/frontend-deployment.yaml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: frontend-deployment
spec:
	replicas: 1
	selector:
		matchLabels:
			app: frontend
	template:
		metadata:
			labels:
				app: frontend
		spec:
			containers:
				- name: frontend
				  image: holahoon/k8s-demo-frontend:latest
```

`k8s/frontend-service.yaml`
```yaml
apiVersion: v1
kind: Service
metadata:
	name: frontend-service
spec:
	selector:
		app: frontend
	type: LoadBalancer
	ports:
		- protocol: TCP
		  port: 80
		  targetPort: 80
```

Let's first build our image and push it to Docker Hub (make sure to have the repository named `k8s-demo-frontend`):
```bash
$ docker build -t holahoon/k8s-demo-frontend ./frontend
$ docker push holahoon/k8s-demo-frontend
```

Then we can start deploying with Deployment and run a Service:
```bash
$ kubectl apply -f ./k8s/frontend-deployment.yaml
$ kubectl apply -f ./k8s/frontend-service.yaml

# Let's start our frontend
$ minikube service frontend-service
```

But, you can quickly notice by running the Service that it fails to fetch.
This is because 

From our frontend application, we need to sent a request to ourselves. Well, to the nginx server which we setup and start in our docker file for the frontend.
We can go into our frontend server configuration to generally server our frontend, but also redirect to some other host/domain if they have a certain structure.
This is the Reverse Proxy concept.

## Using Reverse Proxy

### First step
So, we can go into our nginx configuration:
```conf
server {
	listen 80;

	location /api {
		proxy_pass http://127.0.0.1:54859;
	}

	location / {
	...
}
```
So any request targets `/api` or `/api/...`, this particular configuration will kick in.
Any requests to `/api` will be forwarded to `http://127.0.0.1:54859` address.

In our React application, we can get rid of all the IP address:
```javascript
fetch("/api/tasks", {...})
```

If we start rebuilding the image and push it, then delete the existing Deployment and re-apply to fetch the latest image from Docker Hub, we will quickly notice that our frontend application will fail to find the url.

The problem is that the IP address `http://127.0.0.1:54859` is an address that we can access from our local machine and managed by minikube, but our frontend applications nginx server configuration will not be executed on our local machine. This configuration will run inside of the docker container.
`App.js` basically runs in the browser, so technically runs outside of the Cluster on our local host machine. However, `nginx.conf` code is parsed when the server starts, and that happens inside the Cluster.

We have learned about Cluster Internal IP Addresses([[network between Pods#Using DNS]]).
We can use our automatically generated Domain Names: `tasks-service.default`.
Even though our frontend application code runs inside the browser, not inside the container, we can take advantage of reverse proxy. Because our `nginx.conf` code runs on the Cluster, the domain name will be evaluated and resolved by Kubernetes and will find the `tasks-service` when using this domain.

### Second step
Therefore, instead of the hard coded `tasks-service` IP address, we can use the domain name:
```conf
server {
	listen 80;

	location /api/ {
		proxy_pass tasks-service.default:8000/;
	}

	location / {
	...
}
```
Make sure to add `/` after the `location` and the domain name. This ensures that correct path is later forwarded to the `tasks-service`.
We have also configure in our `k8s/tasks-service.yaml` to expose port `8000`. Therefore, we need to forward our reverse proxy to that port.

We just need to go rebuild the frontend docker image and push. Delete and re-apply the `fronte-deployment` to pick up the changed image.

We now no longer need to manually fetch and enter backend addresses. We can now leverage the reverse proxy concept. 🎉
