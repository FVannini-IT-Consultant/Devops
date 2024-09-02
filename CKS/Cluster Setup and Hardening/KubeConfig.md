Add user to config
```bash
k config  get-users
NAME
kubernetes-admin
```

See [[Certificates]] to obtain key and csr
```bash
k config set-credentials 60099@internal.users --client-key 60099.key --client-certificate 60099.crt
```

```bash
k config  get-users
NAME
60099@internal.users
kubernetes-admin
```

Setting a context
```bash
k config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin

k config get-clusters
NAME
kubernetes
```

```bash
k config set-context 60099@internal.users --cluster=kubernetes --user=60099@internal.users
```

```bash
k config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO               NAMESPACE
          60099@internal.users          kubernetes   60099@internal.users   
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
```

Using a context effectively changing `current-context: kubernetes-admin@kubernetes`

```bash
k config use-context 60099@internal.users
```
which changes to `current-context: 60099@internal.users`

As configuration file `k config view`
```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://172.30.1.2:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: 60099@internal.users
  name: 60099@internal.users
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: 60099@internal.users
  user:
    client-certificate: /root/60099.crt
    client-key: /root/60099.key
- name: kubernetes-admin
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
```

Or cat ~/.kube/config

