https://man7.org/linux/man-pages/man2/syscalls.2.html

To map Linux Syscalls we use strace

```bash
strace kill -9 1234
strace kill -15 1234
strace uname
strace nc -l -p 8080
```

Check an existing PID e.g. kube-apiserver

```bash
ps -ef | grep kube-api
strace -p 2149 -f
strace -p 2149 -f -cw
```

```bash
strace touch /tmp/error.log
pidof etcd
strace -p 3596
```
