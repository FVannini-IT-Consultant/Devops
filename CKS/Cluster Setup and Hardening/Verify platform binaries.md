#node 

Compare SHA hashes of local and online files

```bash
VERSION=$(kubelet --version | cut -d ' ' -f2)
wget https://dl.k8s.io/$VERSION/kubernetes-server-linux-amd64.tar.gz
tar xzf kubernetes-server-linux-amd64.tar.gz
```

```bash
whereis kubelet
sha512sum /usr/bin/kubelet
sha512sum kubernetes/server/bin/kubelet
```
If different one has been manipulated
```bash
sha512sum  /usr/bin/kubelet 
1b3ebe478ec521943b5910084f093a65e1f93aa6c949a941d9f58008e9d594eb859b049e71928ba1020634d6eb7577b7d9751dd26259f4378cf11c3fc96f2d97  /usr/bin/kubelet

sha512sum  kubernetes/server/bin/kubelet 
c09618a8ed80dc57bb58fba16ce579a21db3d2625bf14af29e7a64fd09642761d8b6a007c334284a6abca191ac4e4b0f3064e3e11537a9995dd23342045236d5  kubernetes/server/bin/kubelet
```
