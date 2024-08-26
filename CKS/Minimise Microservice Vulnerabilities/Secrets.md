Secret add env variable
https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/#define-container-environment-variables-with-data-from-multiple-secrets

`k create secret generic holy --from-literal creditcard=1111222233334444`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:  
  containers:
  - image: nginx
    name: pod1
    env:
      - name: HOLY
        valueFrom:
          secretKeyRef:
            name: holy
            key: creditcard
```
Verify
```bash
kubectl exec pod1 -- env | grep "HOLY=1111222233334444"
```

Secret as a mount volume
```yaml
apiVersion: v1
data:
  hosts: MTI3LjAuMC4xCWxvY2FsaG9zdAoxMjcuMC4xLjEJaG9zdDAxCgojIFRoZSBmb2xsb3dpbmcgbGluZXMgYXJlIGRlc2lyYWJsZSBmb3IgSVB2NiBjYXBhYmxlIGhvc3RzCjo6MSAgICAgbG9jYWxob3N0IGlwNi1sb2NhbGhvc3QgaXA2LWxvb3BiYWNrCmZmMDI6OjEgaXA2LWFsbG5vZGVzCmZmMDI6OjIgaXA2LWFsbHJvdXRlcnMKMTI3LjAuMC4xIGhvc3QwMQoxMjcuMC4wLjEgaG9zdDAxCjEyNy4wLjAuMSBob3N0MDEKMTI3LjAuMC4xIGNvbnRyb2xwbGFuZQoxNzIuMTcuMC4zNSBub2RlMDEKMTcyLjE3LjAuMjMgY29udHJvbHBsYW5lCg==
kind: Secret
metadata:
  name: diver
---
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  volumes:
  - name: diver
    secret:
      secretName: diver
  containers:
  - image: nginx
    name: pod1
    volumeMounts:
      - name: diver
        mountPath: /etc/diver
```
Verify
```bash
kubectl exec pod1 -- cat /etc/diver/hosts
```

## Create secrets

From literal
`k create secret  generic sec-a1 -n ns-secure --from-literal=credentials=445566`

From files
`k create secret  generic sec-a2 -n ns-secure --from-file=/etc/hosts`
Check contents
`echo 'MTI3LjAuMC4xIGxvY2FsaG9zdAoKIyBUaGUgZm9sbG93aW5nIGxpbmVzIGFyZSBkZXNpcmFibGUgZm9yIElQdjYgY2FwYWJsZSBob3N0cwo6OjEgaXA2LWxvY2FsaG9zdCBpcDYtbG9vcGJhY2sKZmUwMDo6MCBpcDYtbG9jYWxuZXQKZmYwMDo6MCBpcDYtbWNhc3RwcmVmaXgKZmYwMjo6MSBpcDYtYWxsbm9kZXMKZmYwMjo6MiBpcDYtYWxscm91dGVycwpmZjAyOjozIGlwNi1hbGxob3N0cwoxMjcuMC4wLjEgdWJ1bnR1CjEyNy4wLjAuMSBob3N0MDEKMTI3LjAuMC4xIGNvbnRyb2xwbGFuZQo=' | base64 -d`

Note on Secrets
https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/7431dd03-f5c2-4ebb-b94a-2d35615bbd8c/lesson/cb91290b-c3f5-41a5-af2f-ec2666bf6da6
