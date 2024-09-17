```
docker container prune
docker system prune

docker run --name=c1 base-image -d
docker exec c1 ps
docker stop c1
docker rm c1 --force

docker run -d -p HOST_PORT:CONTAINER_PORT nginx
docker run -d -p 8080:80 nginx
```