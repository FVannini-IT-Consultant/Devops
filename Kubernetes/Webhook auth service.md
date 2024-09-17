```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/authorize', methods=['POST'])
def authorize():
    # For simplicity, allow all requests. You can modify this logic to enforce policies.
    return jsonify({"apiVersion": "authorization.k8s.io/v1",
                    "kind": "SubjectAccessReview",
                    "status": {"allowed": True}})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8443, ssl_context=('webhook-authz-cert.pem', 'webhook-authz-key.pem'))
```

>[!failure] There is a problem in the certificate creation in that it needs a CA and the following has to be updated

san.cnf
```
[ req ]
distinguished_name = req_distinguished_name
req_extensions = req_ext
x509_extensions = v3_req
prompt = no

[ req_distinguished_name ]
CN = webhook-service.kube-system.svc

[ req_ext ]
subjectAltName = @alt_names

[ v3_req ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = webhook-service.kube-system.svc
DNS.2 = webhook-service.kube-system.svc.cluster.local
```

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout webhook-authz.key -out webhook-authz.crt -config san.cnf`

```yaml
kubectl create secret tls webhook-authz-certs \
  --cert=webhook-authz-cert.pem \
  --key=webhook-authz-key.pem \
  -n kube-system
```

```dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY . /app
RUN pip install flask
CMD ["python", "webhook.py"]
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webhook-authz
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webhook-authz
  template:
    metadata:
      labels:
        app: webhook-authz
    spec:
      containers:
      - name: webhook-authz
        image: <your-docker-registry>/webhook-authz:v1
        ports:
        - containerPort: 8443
          name: https
---
#webhook-config.yaml
apiVersion: v1
kind: Config
clusters:
- name: webhook
  cluster:
    certificate-authority: /etc/kubernetes/pki/webhook-ca.crt
    server: https://webhook-service.kube-system.svc:8443/authorize
users:
- name: webhook
  user:
    client-certificate: /etc/kubernetes/pki/webhook-authz-cert.pem
    client-key: /etc/kubernetes/pki/webhook-authz-key.pem
contexts:
- name: webhook
  context:
    cluster: webhook
    user: webhook
current-context: webhook
```

***
`cp webhook-client.crt webhook-client.key /etc/kubernetes/pki`

```
/etc/kubernetes/manifests/kube-apiserver.yaml
- --authorization-mode=Webhook
- --authorization-webhook-config-file=/etc/kubernetes/webhook-config.yaml
...
 volumeMounts:
    - mountPath: /etc/kubernetes/webhook-config.yaml
      name: webhook
      readOnly: true
...
volumes:
  - name: webhook
    hostPath:
      path: /etc/kubernetes/webhook-config.yaml
      type: File
```
