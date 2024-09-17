in `app` namespace
The service
```
web-service.apps
web-service.apps.svc
web-service.apps.svc.cluster.local
```
The pod
```
10-244-2-5.apps
10-244-2-5.apps.pod
10-244-2-5.apps.pod.cluster.local
```

`kubectl run -i --tty --rm debug --image=busybox --restart=Never -- nslookup <hostname>`