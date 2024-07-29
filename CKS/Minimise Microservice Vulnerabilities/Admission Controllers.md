
`NamespaceAutoProvision`

Need to be added to kube-apiserver config

```yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --advertise-address=192.21.249.9
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
```

Check the plugins  enabled/disabled `ps -ef | grep kube-apiserver | grep admission-plugins`

```
--enable-admission-plugins=NodeRestriction,NamespaceAutoProvision
--disable-admission-plugins=DefaultStorageClass
```


