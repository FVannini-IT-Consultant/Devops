https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/

Prepare a secret passkey
```
echo 'this-is-very-sec' | base64
dGhpcy1pcy12ZXJ5LXNlYwo=
```

Create encryption configuration file
`/etc/kubernetes/enc/enc.yaml`

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
--encryption-provider-config=/etc/kubernetes/enc/enc.yaml
...
volumeMounts:
    - mountPath: /etc/kubernetes/enc
      name: enc
      readOnly: true
...
volumes:
  - hostPath:
      path: /etc/kubernetes/enc
      type: DirectoryOrCreate
    name: enc
```