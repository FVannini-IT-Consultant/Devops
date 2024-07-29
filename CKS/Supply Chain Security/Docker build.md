Given this image
```
FROM alpine:3.12.3

RUN adduser -D -g '' appuser

USER appuser

CMD sh -c 'sleep 1d'
```

We can build as (from within the folder)
 `docker build -t base-image .`

Run
`docker run --name c1 -d base-image`

Destroy
`docker rm c1 --force`

## Hardening container

From
```
FROM ubuntu
RUN apt-get update
RUN apt-get -y install curl
ENV URL https://google.com/this-will-fail?secret-token=
CMD ["sh", "-c", "curl --head $URL=2e064aad-3a90-4cde-ad86-16fad1f8943e"]
```

Fix a specific base image version add `:20.04`
```
FROM ubuntu:20.04
RUN apt-get update
RUN apt-get -y install curl
ENV URL https://google.com/this-will-fail?secret-token=
CMD ["sh", "-c", "curl --head $URL=2e064aad-3a90-4cde-ad86-16fad1f8943e"]
```

Ensure consistency at the apt-get caching by consolidating the apt lines in one *layer*
```
FROM ubuntu:20.04
RUN apt-get update && apt-get -y install curl
ENV URL https://google.com/this-will-fail?secret-token=
CMD ["sh", "-c", "curl --head $URL=2e064aad-3a90-4cde-ad86-16fad1f8943e"]
```

Replace hardcode values by using arguments
```
FROM ubuntu:20.04
RUN apt-get update && apt-get -y install curl
ENV URL https://google.com/this-will-fail?secret-token=
CMD ["sh", "-c", "curl --head $URL$TOKEN$"]
```
Then run the container as
```bash
podman run -e TOKEN=2e064aad-3a90-4cde-ad86-16fad1f8943e app
```

Prevent Bash execution
```
FROM ubuntu:20.04
RUN apt-get update && apt-get -y install curl
ENV URL https://google.com/this-will-fail?secret-token=
RUN rm /usr/bin/bash
CMD ["sh", "-c", "curl --head $URL$TOKEN$"]
```