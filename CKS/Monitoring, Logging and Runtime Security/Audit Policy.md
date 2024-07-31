#api-server 
https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/

https://github.com/killer-sh/cks-course-environment/tree/master/course-content/runtime-security/auditing

in kube-apiserver.yaml

>[!warning] To have a new set of policies loaded, the API server should:
- have the current policy line commented out
- wait till the API server restarts
- add the new policy end comment out the line
- wait till the API server restarts

```yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --audit-log-path=/etc/kubernetes/audit-logs/audit.log
    - --audit-log-maxsize=7
    - --audit-log-maxbackup=2
    - --audit-policy-file=/etc/kubernetes/audit-policy/policy.yaml
```

```yaml
volumeMounts:
    - mountPath: /etc/kubernetes/audit-logs
      name: audit-logs
      readOnly: false
    - mountPath: /etc/kubernetes/audit-policy/policy.yaml
      name: audit
      readOnly: true
```
```

```yaml
volumes:
  - hostPath:
      path: /etc/kubernetes/audit-logs
      type: DirectoryOrCreate
    name: audit-logs
  - hostPath:
      path: /etc/kubernetes/audit-policy/policy.yaml
      type: File
```

policy.yaml example

```yaml
# /etc/kubernetes/audit/policy.yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:

# log Secret resources audits, level Metadata
- level: Metadata
  resources:
  - group: ""
    resources: ["secrets"]

# log node related audits, level RequestResponse
- level: RequestResponse
  userGroups: ["system:nodes"]

# for everything else don't log anything
- level: None
- 
```

- Nothing from get, watch and list
- Nothing from events
- Nothing from workers
- Completer for secrets

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"

rules:
# log no "read" actions
- level: None
  verbs: ["get", "watch", "list"]

# log nothing regarding events
- level: None
  resources:
  - group: "" # core
    resources: ["events"]

# log nothing coming from some groups
- level: None
  userGroups: ["system:nodes"]

- level: RequestResponse
  resources:
  - group: ""
    resources: ["secrets"]

# for everything else log
- level: Metadata
```