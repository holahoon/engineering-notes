
Let's say I have a folder that has all the Kubernetes related configurations:
`some-project`
```
├── auth-api
├── users-api
├── ...
├── k8s
│   ├── users-deployment.yaml
│   ├── ...
│   └── ...
└── ...
```

And assume that our current directory set in our Git bash is the `some-project/`. In order to apply the `users-deployment.yaml` k8s configuration:
```bash
$ kubectl apply -f ./k8s/users-deployment.yaml
```
Pretty easy 👏