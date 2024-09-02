- ~~Labs - View Certificates~~
- ~~Labs - Certificate API~~
- ~~Labs - Kubectl Proxy & Port Forward~~
- ~~Labs - Cluster upgrade~~
- ~~Labs - Ingress - 1~~
- ~~Labs - Ingress - 2~~

Article: Ingress
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/eac6dac8-4481-4138-96ef-a2135f20e05e/lesson/42cf821c-38ef-4cd5-abce-dfcaf0250c11
Article: Ingress - Annotations and rewrite-target
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/eac6dac8-4481-4138-96ef-a2135f20e05e/lesson/416fc31d-8436-4fb2-be44-8d29bee19334

Article: Securing Control Plane Communications with Ciphers
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/eac6dac8-4481-4138-96ef-a2135f20e05e/lesson/3e7ad1e6-8aa3-476b-8e26-7c48659ae91e

- ~~Lab - Identify open ports, remove packages services~~
- ~~Lab - UFW Firewall~~
- ~~Lab - Pod Security Admission.~~

~~Article: Understanding Pod Security Policy~~
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/7431dd03-f5c2-4ebb-b94a-2d35615bbd8c/lesson/a3ca9b49-2589-4497-afb1-213b9c4dc47d

- ~~Labs - OPA~~
- ~~Labs - OPA in Kubernetes~~
~~- Lab - Manage Kubernetes secrets~~

Article - Note on Secrets
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/7431dd03-f5c2-4ebb-b94a-2d35615bbd8c/lesson/cb91290b-c3f5-41a5-af2f-ec2666bf6da6

~~- Labs - Image Security~~
~~- Labs - Ensure Immutability of Containers at Runtime~~

## From Killer.sh simulator
- Q2
falco rules
crictl ps -id 7a5ea6a080d1
crictl pods -id 7a864406b9794

- Q5 more speed!

- Q8
Where is documentation?

- Q12
Documentation about curl kube-api

- Q13
Try to recreate scenario and use ingress and egress instead

- Q14
Syscall and strace

- Q19
Immutable Root FS

---
practice EOF

practice jq

## From first attempt

- NodeRestriction
- ServiceAccount limiting
- Audit rules
- Falco rules
- Sysdig
- Dockerfiles best-practices
- Secrets mount in pods
 - ImagePolicyWebhook
 - Restrict Communication (API-Server)
 - Restrict Communication from Kubelet to API-Server
https://medium.com/@artem_lajko/cks-evolution-2021-2023-60699c879886

