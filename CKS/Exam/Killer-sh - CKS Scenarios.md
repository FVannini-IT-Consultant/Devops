https://github.com/killer-sh/cks-course-environment/blob/master/Scenarios.md

# Killercoda Scenarios

Many topics in the CKS course have interactive in-browser Killercoda scenarios at the end. Solve these to test and harden your knowledge!

|     | Topic                                                               |     |
| --- | ------------------------------------------------------------------- | --- |
| 1   | Foundation - Containers under the hood                              | o   |
| 2   | Cluster Setup - Network Policies                                    | o   |
| 3   | Cluster Setup - Secure Ingress                                      | o   |
| 4   | Cluster Setup - Node Metadata Protection                            | o   |
| 5   | Cluster Setup - CIS Benchmarks                                      | o   |
| 6   | Cluster Setup - Verify Platform Binaries                            | o   |
| 7   | Cluster Hardening - RBAC                                            | o   |
| 8   | Cluster Hardening - Exercise caution in using ServiceAccounts       | o   |
| 9   | Cluster Hardening - Restrict API Access                             | o   |
| 10  | Microservice Vulnerabilities - Manage Kubernetes Secrets            | o   |
| 11  | Microservice Vulnerabilities - Container Runtime Sandboxes          | o   |
| 12  | Microservice Vulnerabilities - OS Level Security Domains            | o   |
| 13  | Supply Chain Security - Image Footprint                             | o   |
| 14  | Supply Chain Security - Static Analysis                             | o   |
| 15  | Supply Chain Security - Image Vulnerability Scanning                | o   |
| 16  | Supply Chain Security - Secure Supply Chain                         | o   |
| 17  | Runtime Security - Behavioral Analytics at host and container level | o   |
| 18  | Runtime Security - Immutability of containers at runtime            | o   |
| 19  | Runtime Security - Auditing                                         | o   |
| 20  | System Hardening - Kernel Hardening Tools                           | o   |
| 21  | System Hardening - Reduce Attack Surface                            | o   |

- 8 Cluster Hardening - Exercise caution in using ServiceAccounts
https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#serviceaccount-admission-controller
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

- 18 Runtime Security - Immutability of containers at runtime
  emptyDir configuration example https://kubernetes.io/docs/concepts/storage/volumes/#emptydir
  Configure a Security Context for a Pod or Container https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
- 19 https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/#log-backend

## Cluster creation
https://github.com/killer-sh/cks-course-environment/tree/master/cluster-setup/latest

`curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh -o install_master.sh`

`curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh -o install_worker.sh`

NIC setup for VBox linked clones
https://www.freecodecamp.org/news/setting-a-static-ip-in-ubuntu-linux-ip-address-tutorial/
```
/etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.64.40/24
      gateway4: 192.168.64.1
      nameservers:
        addresses:
          - 192.168.64.1
```