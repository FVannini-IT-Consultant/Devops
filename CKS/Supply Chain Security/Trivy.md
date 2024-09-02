trivy #container
Vulnerability scanner for containers and Kubernetes. 
_Example: Scans container images to find known vulnerabilities in software packages._
**Needs**: Standalone CLI tool or integrated into CI/CD pipelines, scans container images.

https://github.com/aquasecurity/trivy#docker

Run a scan for nginx latest
`docker run ghcr.io/aquasecurity/trivy:latest image nginx:latest`

For a specific version
`docker run ghcr.io/aquasecurity/trivy:latest image nginx:1.19.1-alpine-perl`

Check a specific vulnerability
`docker run ghcr.io/aquasecurity/trivy:latest image httpd:2.4.39-alpine | grep CVE-2016-9841`

If Trivy is installed it can be run as
````bash
trivy image nginx:1.19.1-alpine-perl | grep CVE-2021-28831

trivy image --severity CRITICAL kodekloud/webapp-delayed-start

trivy k8s --include-namespaces delta --report summary
```

`kubectl run -i --rm trivy --image=aquasec/trivy --command -- sh -c "trivy image nginx:alpine --severity=CRITICAL | grep Total:"`