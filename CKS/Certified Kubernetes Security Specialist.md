## Cluster Setup and Hardening

| Topic                                 | Tag                     |
| ------------------------------------- | ----------------------- |
| [[CIS Benchmark]] kube-bench          | #node #etcd #api-server |
| Authentication-Authorisation          |                         |
| [[Service Accounts]]                  | #service-account        |
| TLS                                   | #node                   |
| [[Certificates]]                      |                         |
| [[KubeConfig]]                        |                         |
| [[Accessing the REST API]] API Groups | #api-server             |
| [[Restrict API Access]]               | #api-server             |
| [[RBAC]]                              | #rbac                   |
| [[Kubelet security]]                  | #kubelet                |
| [[Securing Kubernetes dashboard]]     | #kube-dashboard         |
| [[Verify platform binaries]]          | #node                   |
| K8s SW version                        |                         |
| Cluster upgrade process               | #node                   |
| [[Network policies]]                  | #service #pod           |
| [[Ingress]]                           | #service                |

## System Hardening

| Topic                                                                                          | Tag        |
| ---------------------------------------------------------------------------------------------- | ---------- |
| [[Seccomp]] (Secure Computing Mode) works by restricting the system calls                      | #container |
| [[AppArmor]] provides a granular level of security by enforcing mandatory access control (MAC) | #container |
| Least privilege                                                                                |            |
| Limit node access                                                                              |            |
| [[SSH hardening]]                                                                              | #node      |
| Obsolete packages and services                                                                 | #node      |
| Kernel modules                                                                                 | #node      |
| [[Disable open ports]]                                                                         | #node      |
| Minimise IAM roles                                                                             |            |
| [[UFW Firewall]]                                                                               | #node      |
| [[Syscalls]]                                                                                   | #pod       |
| [[Tracee]]                                                                                     |            |

## Minimise Microservice Vulnerabilities

| Topic                                    | Tag                              |
| ---------------------------------------- | -------------------------------- |
| [[Security Contexts]]                    | #pod #container                  |
| [[Admission Controllers]]                | #container                       |
| [[Dynamic Admission Control]]            | #pod #admissioncontroller        |
| [[Pod Security Policies]]                | #pod #admissioncontroller        |
| [[OPA]]                                  |                                  |
| [[OPA Gatekeeper]]                       | #pod #image #admissioncontroller |
| [[Secrets]]                              | #secret #configmap #etcd         |
| [[gVisor]]                               |                                  |
| kata Containers                          |                                  |
| Runtime classes                          |                                  |
| SSL One way - Mutual                     |                                  |
| [[Encrypting Confidential Data at Rest]] | #etcd                            |

## Supply Chain Security

| Topic                                 | Tag                                     |
| ------------------------------------- | --------------------------------------- |
| Image Security - [[Docker build]]     | #image                                  |
| Image Security - [[Private Registry]] | #image                                  |
| Registries                            | #image                                  |
| Static Analysis                       | #container                              |
| [[Kubesec]]                           | #pod #deployment #service               |
| [[Trivy]]                             | #pod #container #deployment #replicaset |
| [[ImagePolicyWebhook]]                | #admissioncontroller                    |
## Monitoring, Logging and Runtime Security

| Topic            | Tag             |
| ---------------- | --------------- |
| [[Audit Policy]] | #pod #nodes     |
| Falco            | #pod #container |
