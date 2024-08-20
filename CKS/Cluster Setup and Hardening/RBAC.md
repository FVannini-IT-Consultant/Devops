#rbac 

Create a serviceaccount that can view resources in the cluster
```bash
k create serviceaccount -n ns1 pipeline
k create serviceaccount -n ns2 pipeline

k create clusterrolebinding ns1-pipeline-view --clusterrole=view --serviceaccount=ns1:pipeline
k create clusterrolebinding ns2-pipeline-view --clusterrole=view --serviceaccount=ns2:pipeline

k auth can-i get deployment --as system:serviceaccount:ns1:pipeline
k auth can-i get deployment --as system:serviceaccount:ns2:pipeline
```

Create a serviceaccount that can create and delete deployments in its namespace
```bash
k create -n ns1 role deployment-manager --verb=create,delete --resource=deployments
k create -n ns1 rolebinding pipeline-deployment-manager --role=deployment-manager --serviceaccount=ns1:pipeline

k -n ns1 auth can-i create deployment --as=system:serviceaccount:ns1:pipeline

k create -n ns2 role deployment-manager --verb=create,delete --resource=deployments
k create -n ns2 rolebinding pipeline-deployment-manager --role=deployment-manager --serviceaccount=ns2:pipeline

k -n ns2 auth can-i create deployment --as=system:serviceaccount:ns2:pipeline
```

Create a role like clusterrole view but exclude one namespace (e.g. kube-system). The only way to do it is to create a rolebinding for all other ns.
```bash
k get ns
NAME                 STATUS   AGE
applications         Active   75s
default              Active   12d
kube-node-lease      Active   12d
kube-public          Active   12d
kube-system          Active   12d
local-path-storage   Active   12d

k create rolebinding -n applications smoke-view --clusterrole=view --user=smoke
k create rolebinding -n default smoke-view --clusterrole=view --user=smoke
k create rolebinding -n kube-node-lease smoke-view --clusterrole=view --user=smoke
k create rolebinding -n kube-public smoke-view --clusterrole=view --user=smoke
```

To test a normal user
```bash
k auth can-i get pod --user martin -n dev-z
yes

k auth can-i delete pod --user martin -n dev-z
no
```