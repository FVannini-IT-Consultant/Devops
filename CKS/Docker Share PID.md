From [[Killer-sh - CKS Scenarios]]
```
docker run --name app1 -d nginx:alpine sleep infinity
docker exec app1 ps aux
docker run --name app3 -d nginx:alpine sleep infinity
docker run --name app2 --pid=container:app1 -d nginx:alpine sleep infinity
docker exec app2 ps aux
docker exec app1 ps aux
docker exec app3 ps aux
```

```
controlplane $ docker exec app2 ps aux
PID   USER     TIME  COMMAND
    1 root      0:00 sleep infinity
   27 root      0:00 sleep infinity
   32 root      0:00 ps aux
controlplane $ docker exec app1 ps aux
PID   USER     TIME  COMMAND
    1 root      0:00 sleep infinity
   27 root      0:00 sleep infinity
   37 root      0:00 ps aux
controlplane $ docker exec app3 ps aux
PID   USER     TIME  COMMAND
    1 root      0:00 sleep infinity
    7 root      0:00 ps aux
```