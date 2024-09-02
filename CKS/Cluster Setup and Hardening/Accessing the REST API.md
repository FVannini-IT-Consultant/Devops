https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/

`kubectl proxy` - Opens proxy port to API server

Establish a proxy connection
`kubectl proxy --port=8080 &`

Query directly the API
`curl localhost:8001/version`

`curl localhost:8001/api/v1/namespaces/default/secrets | jq -j '.items[].data.password' | base64 -d`

> [!Note] use **`/api`** - for core API groups, such as retrieving Pods, Nodes, etc. **`/apis`** for other API groups, such as apps, batch, extensions, etc.

`curl localhost:8001/api/v1/namespaces/default/pods`
`curl localhost:8001/apis/apps/v1/namespaces/default/deployments`

### Without the proxy the above will generally require a token
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_create/kubectl_create_token/
`kubectl create token myapp`

Hence
`curl https://kubernetes.default/api/v1/namespaces/restricted/secrets -H "Authorization: Bearer $(cat /<token path>)" -k`
See [[Service Accounts]]

_-k for insecure connection_
_-H specifies HTTP Header_

## Port Forward
`kubectl port-forward` - Opens port to target deployment and pods

Listen on port 8888 locally, forwarding to 5000 in the pod
`kubectl port-forward pod/mypod 8888:5000`

Deployment
`kubectl port-forward deployments/nginx 8005:80`

## Using certificates

### From controlplane
```bash
curl https://localhost:6443 -k \
--key /etc/kubernetes/pki/apiserver-kubelet-client.key \
--cert /etc/kubernetes/pki/apiserver-kubelet-client.crt \
--cacert /etc/kubernetes/pki/ca.crt
```
### From desktop
`k config view` use `--raw` to view certs
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://192.168.64.40:6443
  name: kubernetes
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
- context:
    cluster: kubernetes
    namespace: metadata-access
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
- name: kubernetes-admin
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
```
Then create files for ca, crt and key by looking at `~/.kube/config`

For each file `echo <base64encoded> | base64 -d > <filename>`
e.g. `echo LS0tLS1CRUdJTiBDRVJUS... | base64-d > ca`

`curl https://192.168.64.40:6443` on its own cannot establish a connection
```
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```
`curl https://192.168.64.40:6443 --cacert ca` Already trusting the CA yields a different result although forbidden
```yaml
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
This would allow to trust and authenticate
 `curl https://192.168.64.40:6443 --cacert ca --cert crt --key key`

```json
{
  "paths": [
    "/.well-known/openid-configuration",
    "/api",
    "/api/v1",
    "/apis",
    "/apis/"
    ...
```

#### Using config file from controlplane

The config cannot be used because the certificate is valid only for internal IP addresses. We can still connect using the names specified in the certificate simply by adding them in /etc/hosts on the client.

The first thing is to **change the service for the kube-api server** from ClusterIP to NodePort

On the controlplane in fact
`penssl x509 -in /etc/kubernetes/pki/apiserver.crt --noout --text` 
```
...
DNS:cks-master, DNS:kubernetes, DNS:kubernetes.default, DNS:kubernetes.default.svc, DNS:kubernetes.default.svc.cluster.local, IP Address:10.96.0.1, IP Address:192.168.64.40
...
```

On the client the `/etc/hosts`
```
# This file was automatically generated by WSL. To stop automatic generation of this file, add the following entry to /etc/wsl.conf:
# [network]
# generateHosts = false
127.0.0.1       localhost
127.0.1.1       Poseidon.       Poseidon
192.168.64.40   cks-master
```
and the kube config file
```
...
 server: https://cks-master:6443
...
```
we can then
`kubectl --kubeconfig remote-kubeconfig get po`