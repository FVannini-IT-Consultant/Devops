#node #etcd #api-server 
Security configuration guidelines for Kubernetes. 
_Example: Provides a checklist to ensure your Kubernetes setup follows industry-standard security practices._

It is a benchmark to provide a security baseline. It works for:
- OS
- Cloud
- Devices
- Network
- Software
- etc

https://www.cisecurity.org/cis-benchmarks

CIS Benchmark assessment tools:
- CIS CAT Pro https://www.cisecurity.org/cybersecurity-tools/cis-cat-pro/cis-benchmarks-supported-by-cis-cat-pro
- CIS CAT Lite https://learn.cisecurity.org/cis-cat-lite
```bash
cd /root/Assessor
sh ./Assessor-CLI.sh -i -rd /var/www/html/ -nts -rp index
```
- Kube-bench from aqua https://aquasecurity.github.io/kube-bench/v0.6.15/
```bash
curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.4.0/kube-bench_0.4.0_linux_amd64.tar.gz -o kube-bench_0.4.0_linux_amd64.tar.gz
tar -xvf kube-bench_0.4.0_linux_amd64.tar.gz

./kube-bench --config-dir `pwd`/cfg --config `pwd`/cfg/config.yaml
```

Run benchmark against a specific resource `kube-bench run --targets master --check="1.3.2"`

- **master**: Master node components such as the API server, controller manager, and scheduler.
- **node**: Worker node components such as kubelet and kube-proxy.
- **etcd**: etcd key-value store.
- **controlplane**: Entire control plane components (similar to `master` but may encompass additional checks depending on the benchmark version and configuration).
- **policies**: Policies, such as network policies or Pod Security Policies.

Course material:
- Killer Shell - Cluster Setup - CIS Benchmarks  https://www.youtube.com/watch?v=d9xfB5qaOfg&t=9253s- 