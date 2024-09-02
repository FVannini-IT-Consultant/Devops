In a deployment or pod where an authenticated repository is to be used
```yaml
...
spec:
      containers:
      - image: myprivateregistry.com:5000/nginx:alpine
...
```

Credentials need to be provided via a secret
`kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL`

Which is then used as
```yaml
...
spec:
      containers:
      - image: myprivateregistry.com:5000/nginx:alpine
      imagePullSecrets:
      - name: my-secret
...
```
