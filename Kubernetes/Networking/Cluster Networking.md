https://kubernetes.io/docs/concepts/cluster-administration/networking/#:~:text=Kubernetes%20IP%20address%20ranges&text=The%20network%20plugin%20is%20configured,assign%20IP%20addresses%20to%20Nodes.


![[kubernetes-cluster-network_1_.png]]

- Nodes  `kubectl get nodes -o wide` 
```
NAME           STATUS   ROLES           AGE     VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
controlplane   Ready    control-plane   2d15h   v1.30.0   172.30.1.2    <none>        Ubuntu 20.04.5 LTS   5.4.0-131-generic   containerd://1.7.13
```
*The kubelet or the cloud-controller-manager is configured to assign IP addresses to Nodes.*

- Services `kubectl get service
```
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   2d15h
nginx        ClusterIP   10.105.22.115   <none>        80/TCP    6s
```
*The kube-apiserver is configured to assign IP addresses to Services.*
`cat /etc/kubernetes/manifests/kube-apiserver.yaml`
```
...
- --service-cluster-ip-range=10.96.0.0/12
...
```

- Pods `Kubectl get pods -o wide`
```
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          27s   10.244.1.2   node01   <none>           <none>
```
*The network plugin is configured to assign IP addresses to Pods.*
`ip -4 address`

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: flannel.1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue state UNKNOWN group default 
    inet 10.244.0.0/32 brd 10.244.0.0 scope global flannel.1
       valid_lft forever preferred_lft forever
3: cni0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue state UP group default qlen 1000
    inet 10.244.0.1/24 brd 10.244.0.255 scope global cni0
       valid_lft forever preferred_lft forever
```

**Other approach**
`kubectl get configmap kubeadm-config -n kube-system -o yaml`
```
...
networking:
      dnsDomain: cluster.local
      podSubnet: 10.244.0.0/16
      serviceSubnet: 10.96.0.0/12
...
```
