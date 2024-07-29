https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

Create encryption configuration file
`/etc/kubernetes/etcd/ec.yaml`

This example ensures:
- new secrets are encrypted `aesgcm`
- non-encrypted ones can still be read `identity`

```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - identity: {} # plain text, in other words NO encryption
      - aesgcm:
          keys:
            - name: key1
              secret: dGhpcy1pcy12ZXJ5LXNlYw==
```

Modify kube-apiserver tp use the file above by adding
```yaml
--encryption-provider-config=/etc/kubernetes/etcd/ec.yaml
...
volumeMounts:
    - mountPath: /etc/kubernetes/etcd
      name: etcd
      readOnly: true
...
volumes:
  - hostPath:
      path: /etc/kubernetes/etcd
      type: DirectoryOrCreate
    name: etcd
```