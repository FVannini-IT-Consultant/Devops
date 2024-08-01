#container 
Sandbox for container isolation. 
_Example: Runs containers with additional security boundaries, preventing them from accessing the host system directly._

Create runtimeclass
https://kubernetes.io/docs/concepts/containers/runtime-class/#2-create-the-corresponding-runtimeclass-resources
```yaml
# RuntimeClass is defined in the node.k8s.io API group
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  # The name the RuntimeClass will be referenced by.
  # RuntimeClass is a non-namespaced resource.
  name: gvisor
# The name of the corresponding CRI configuration
handler: runsc
```

Apply to Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: sec
  name: sec
spec:
  runtimeClassName: gvisor
  containers:
  - image: nginx:1.21.5-alpine
    name: sec
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```