`project-structure`
```
├── auth-api
│	├── ...
│	├── Dockerfile
│	└── auth-app.js
├── users-api
│	├── ...
│	├── Dockerfile
│	└── users-app.js
├── tasks-api
│	├── ...
│	├── tasks
│	│	└── tasks.txt
│	├── Dockerfile
│	└── tasks-app.js
├── k8s
│   ├── users-deployment.yaml
│   ├── users-service.yaml
│   └── ...
├── docker-compose.yaml
└── ...
```

## `users-api`

`users-api/users-app.js`
```javascript
...
app.post("/signup", async (req, res) => {
...
	try {
		const hashedPW = await axios.get("http://auth/hashed-password/" + password);
		// since it's a dummy service, we don't really care for the hashed-pw either
		console.log(hashedPW, email);
		res.status(201).json({ message: "User created!" });
	} catch (err) {
		console.log(err);
		return res
			.status(500)
			.json({ message: "Creating the user failed - please try again later." });
	}
});

...

app.post("/login", async (req, res) => {
...
	const hashedPassword = password + "_hash";
	const response = await axios.get(
		"http://auth/token/" + hashedPassword + "/" + password
	);
	if (response.status === 200) {
		return res.status(200).json({ token: response.data.token });
	}
	return res.status(response.status).json({ message: "Logging in failed!" });
});

app.listen(8080);
```

`Dockerfile`
```dockerfile
FROM node:14-alpine

WORKDIR /app  

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD [ "node", "auth-app.js" ]
```

## `auth-api`

`auth-api/auth-app.js`
```javascript
...
app.get('/verify-token/:token', (req, res) => {
	const token = req.params.token;
	
	// dummy verification!
	if (token === 'abc') {
		return res.status(200).json({ message: 'Valid token.', uid: 'u1' });
	}
		res.status(401).json({ message: 'Token invalid.' });
	});

app.get('/token/:hashedPassword/:enteredPassword', (req, res) => {
	const hashedPassword = req.params.hashedPassword;
	const enteredPassword = req.params.enteredPassword;
	// dummy password verification!
	if (hashedPassword === enteredPassword + '_hash') {
		const token = 'abc';
		return res.status(200).json({ message: 'Token created.', token: token });
	}
	res.status(401).json({ message: 'Passwords do not match.' });
});

app.get('/hashed-password/:password', (req, res) => {
	// dummy hashed pw generation!
	const enteredPassword = req.params.password;
	const hashedPassword = enteredPassword + '_hash';
	res.status(200).json({ hashedPassword: hashedPassword });
});

app.listen(80);
```

`Dockerfile`
```dockerfile
FROM node:14-alpine

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 8080  

CMD [ "node", "auth-app.js" ]
```

## `Tasks-api`

```javascript
...
const filePath = path.join(__dirname, process.env.TASKS_FOLDER, "tasks.txt");
...

const extractAndVerifyToken = async (headers) => {
	if (!headers.authorization) {
		throw new Error("No token provided.");
	}
	const token = headers.authorization.split(" ")[1]; // expects Bearer TOKEN
	const response = await axios.get("http://auth/verify-token/" + token);
	return response.data.uid;
};

app.get("/tasks", async (req, res) => {
	try {
		// we don't really need the uid
		const uid = await extractAndVerifyToken(req.headers); 
		fs.readFile(filePath, (err, data) => {
			if (err) {
				console.log(err);
				return res.status(500).json({ message: "Loading the tasks failed." });
			}
			const strData = data.toString();
			const entries = strData.split("TASK_SPLIT");
			entries.pop(); // remove last, empty entry
			console.log(entries);
			const tasks = entries.map((json) => JSON.parse(json));
			res.status(200).json({ message: "Tasks loaded.", tasks: tasks });
		});
	} catch (err) {
		console.log(err);
		return res
			.status(401)
			.json({ message: err.message || "Failed to load tasks." });
	}
});

app.post("/tasks", async (req, res) => {
	try {
		// we don't really need the uid
		const uid = await extractAndVerifyToken(req.headers);
		const text = req.body.text;
		const title = req.body.title;
		const task = { title, text };
		const jsonTask = JSON.stringify(task);
		fs.appendFile(filePath, jsonTask + "TASK_SPLIT", (err) => {
			if (err) {
				console.log(err);
				return res.status(500).json({ message: "Storing the task failed." });
			}
			res.status(201).json({ message: "Task stored.", createdTask: task });
		});
	} catch (err) {
		return res.status(401).json({ message: "Could not verify token." });
	}
});

app.listen(8000);
```

`Dockerfile`
```dockerfile
FROM node:14-alpine

WORKDIR /app  

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

CMD [ "node", "tasks-app.js" ]
```
## Docker compose

`docker-compose.yaml`
```yaml
version: "3"

services:
	auth:
		build: ./auth-api
	users:
		build: ./users-api
		ports:
			- "8080:8080"
		environment:
			AUTH_ADDRESS: auth
	tasks:
		build: ./tasks-api
		ports:
			- "8000:8000"
		environment:
			TASKS_FOLDER: tasks
```
