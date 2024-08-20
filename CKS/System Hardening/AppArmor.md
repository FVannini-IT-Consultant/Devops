#pod #container
Linux kernel security module for access control.
_Example: Restricts a container's ability to access certain files or network resources._
~~It is currently in Beta for K8s~~
**Needs**: AppArmor profiles, Kubernetes pod securityContext section in YAML.

Default directory `/etc/apparmor.d/`
>[!important] Each node has to have a copy of the profile

```
aa-status
aa-genprof
aa-complain
aa-enforce
aa-logprof
```

### For a program
`apt-get install apparmor-utils`

e.g. `aa-genprof curl` then F. This will block curl from doing anything
`aa-logprof` to allow curl again on all or certain actions

### For a container

Load a profile `apparmor_parser -q /etc/apparmor.d/usr.sbin.nginx`

Example profile

```yaml
#include <tunables/global>

profile custom-nginx flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>

  network inet tcp,
  network inet udp,
  network inet icmp,

  deny network raw,

  deny network packet,

  file,
  umount,

  deny /bin/** wl,
  deny /boot/** wl,
  deny /dev/** wl,
  deny /etc/** wl,
  deny /home/** wl,
  deny /lib/** wl,
  deny /lib64/** wl,
  deny /media/** wl,
  deny /mnt/** wl,
  deny /opt/** wl,
  deny /proc/** wl,
  deny /root/** wl,
  deny /sbin/** wl,
  deny /srv/** wl,
  deny /tmp/** wl,
  deny /sys/** wl,
  deny /usr/** wl,

  audit /** w,

  /var/run/nginx.pid w,

  /usr/sbin/nginx ix,

  deny /bin/dash mrwklx,
  deny /bin/sh mrwklx,
  deny /usr/bin/top mrwklx,


  capability chown,
  capability dac_override,
  capability setuid,
  capability setgid,
  capability net_bind_service,

  deny @{PROC}/{*,**^[0-9*],sys/kernel/shm*} wkx,
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/mem rwklx,
  deny @{PROC}/kmem rwklx,
  deny @{PROC}/kcore rwklx,
  deny mount,
  deny /sys/[^f]*/** wklx,
  deny /sys/f[^s]*/** wklx,
  deny /sys/fs/[^c]*/** wklx,
  deny /sys/fs/c[^g]*/** wklx,
  deny /sys/fs/cg[^r]*/** wklx,
  deny /sys/firmware/efi/efivars/** rwklx,
  deny /sys/kernel/security/** rwklx,
}
```

Example to restrict an additional folder `deny /usr/share/nginx/html/restricted/* rw,` 

Pod definition
>[!Warning] Annotations are no longer needed as of v1.30

So this
```
container.apparmor.security.beta.kubernetes.io/<container_name>: localhost/<profile_name>
```
is replaced within the container as
```yaml
securityContext:
  appArmorProfile:
    type: <profile_type>
    localhostProfile: <profile_name>
```

```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    container.apparmor.security.beta.kubernetes.io/nginx: localhost/restricted-nginx
  labels:
    run: nginx
  name: nginx
  namespace: default
  spec:
  containers:
  - image: nginx:alpine
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    securityContext:
      appArmorProfile:
        localhostProfile: restricted-nginx
        type: Localhost
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: test-volume
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-nqmb2
      readOnly: true
```



