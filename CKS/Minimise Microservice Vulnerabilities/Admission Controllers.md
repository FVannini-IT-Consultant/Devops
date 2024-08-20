>An _admission controller_ is a piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object, but after the request is authenticated and authorized. Admission controllers may be _validating_, _mutating_, or both. Mutating controllers may modify objects related to the requests they admit; validating controllers may not.

An example of admission controller is `NamespaceAutoProvision`

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


