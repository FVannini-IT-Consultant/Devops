https://kubernetes.io/docs/concepts/security/controlling-access/
## Anonymous access
From a controlplane
`curl https://localhost:6443 -k`
```json
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```
it should not respond!

add
`--anonymous-auth=false` to kube-apiserver.yaml
which yields
```json
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```

## Insecure access

> [!warning] This has now been removed since 1.20

`--insecure-port`
- If set to 0 it's disabled
- Otherwise it's enabled and bypasses authentication and authorisation!! Only use for debug

`--insecure-port=8080`

`curl http://localhost:8080` HTTP on 8080

NodeRestriction
>[!Note] Enabled by default

https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction

> [!quote] This admission controller limits the `Node` and `Pod` objects a kubelet can modify

On a node for example even though we can use the kubelet kubeconfig file to interact with the Kube Api
`kubectl --kubeconfig=/etc/kubernetes/kubelet.conf  get po`

We can't change for example another node's label
```bash
kubectl --kubeconfig=/etc/kubernetes/kubelet.conf label node cks-worker2 test=test

Error from server (Forbidden): nodes "cks-worker2" is forbidden: node "cks-worker1" is not allowed to modify node "cks-worker2"
```