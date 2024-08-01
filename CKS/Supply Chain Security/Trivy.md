#container
Vulnerability scanner for containers and Kubernetes. 
_Example: Scans container images to find known vulnerabilities in software packages._

https://github.com/aquasecurity/trivy#docker

Run a scan for nginx latest
`docker run ghcr.io/aquasecurity/trivy:latest image nginx:latest`

For a specific version
`docker run ghcr.io/aquasecurity/trivy:latest image nginx:1.19.1-alpine-perl`

Check a specific vulnerability
`docker run ghcr.io/aquasecurity/trivy:latest image httpd:2.4.39-alpine | grep CVE-2016-9841`

If Trivy is installed it can be run as
````yaml
trivy image nginx:1.19.1-alpine-perl | grep CVE-2021-28831
````