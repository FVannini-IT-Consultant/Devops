https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

Both at the pod or container levels
```yaml
securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
```

Run a container as non-root. **Needs something like above** to verify the user will not run as root or that the base Docker image is set to run as a different user than root.
```yaml
securityContext:
  runAsNonRoot: true
```

At the pod level
```yaml
securityContext:
  capabilities:
    add: ["NET_ADMIN", "SYS_TIME"]
```

### Privileged
Privileged `true` or unprivileged(default) `false` pod. At the level of the pod or container (Not always e.g. sts only work at container)
> [!warning] It means user 0 (root) in container is mapped to user 0 (root) on host 
```yaml
securityContext:
  privileged: true
```
A command like `sysctl kernel.hostname=hacker` will now work!

A process can gain more privileges than its parent process
If `true` (default) `cat /proc/1/status` shows `NoNewPrivs:0`
```yaml
securityContext:
      allowPrivilegeEscalation: false
```

### ReadOnlyFileSystem
```yaml
securityContext:
  readOnlyRootFilesystem: true
```

This will probably require empytDir ([generic ephemeral volumes](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/#generic-ephemeral-volumes))
```yaml
volumeMounts:
    - mountPath: /cache
      name: cache-volume
  volumes:
  - name: cache-volume
    emptyDir:
      sizeLimit: 500Mi
```