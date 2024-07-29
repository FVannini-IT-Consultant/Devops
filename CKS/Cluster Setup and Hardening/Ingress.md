#service 

Imperative service with Exact path type ` kubectl create ingress world --rule="world.universe.mine/europe=asia:80"`

Imperative service with Prefix path type ` kubectl create ingress world --rule="world.universe.mine/europe*=asia:80"`

Imperative service with Prefix, target rewrite and namespace
```
kubectl create ingress world \
--rule="world.universe.mine/europe*=europe:80" \
--rule="world.universe.mine/asia*=asia:80" \
--annotation "nginx.ingress.kubernetes.io/rewrite-target=/" \
--class=nginx \
-n world
```

Secure with TLS

- Request certificate and key
`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt -subj "/CN=world.universe.mine/O=world.universe.mine"`

- Create secret
`kubectl create secret tls tls-secret --cert=cert.crt --key=cert.key -n world`

- Edit ingress
```yaml
apiVersion: v1
items:
- apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /$1
      nginx.ingress.kubernetes.io/ssl-redirect: "false"
      nginx.ingress.kubernetes.io/use-regex: "true"
    name: world
    namespace: world
  spec:
    ingressClassName: nginx
    rules:
    - host: world.universe.mine
      http:
        paths:
        - backend:
            service:
              name: europe
              port:
                number: 80
          path: /europe
          pathType: Prefix
        - backend:
            service:
              name: asia
              port:
                number: 80
          path: /asia
          pathType: Prefix
    tls:
    - hosts:
      - world.universe.mine
      secretName: tls-secret
  status:
    loadBalancer:
      ingress:
      - ip: 172.30.1.2
kind: List
metadata:
  resourceVersion: ""
```

Where the following was added
```
tls:
    - hosts:
      - world.universe.mine
      secretName: tls-secret
```
- Verify
`curl -kv https://world.universe.mine:30443/europe`

- Bonus: One liner imperative
```
kubectl create ingress world \
--rule="world.universe.mine/europe*=europe:80,tls=tls-secret" \
--rule="world.universe.mine/asia*=asia:80,tls=tls-secret" \
--annotation "nginx.ingress.kubernetes.io/rewrite-target=/" \
--class=nginx \
-n world
```
