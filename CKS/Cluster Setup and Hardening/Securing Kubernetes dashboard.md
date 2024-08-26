https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```bash
# Add kubernetes-dashboard repository
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
# Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

To access via a proxy we need:

- serviceaccount
- cluster-rolebinding
- token

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

```bash
kubectl -n kubernetes-dashboard create token admin-user
```
Then use the token in the GUI

>[!INFO] The old way `kubectl get secrets -n kubernetes-dashboard admin-user -o go-template="{{.data.token | base64decode}}"`

Access dashboard
` kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443`
Where 8443 is to be used in the browser e.g. `https://localhost:8443`