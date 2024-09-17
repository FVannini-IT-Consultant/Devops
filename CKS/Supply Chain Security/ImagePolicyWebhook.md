#admissioncontroller
*Validating admission plugin*

This is a type of [[Dynamic Admission Control]]

To enable it
`kube-apiserver --enable-admission-plugins=NodeRestriction,ImagePolicyWebhook`

It **requires** a kubeconfig file and the relative volume mount in the kube-api manifest file.
>[!info] The kubeconfig file is needed especially if the webhook server is running outside of the Kubernetes cluster or needs to authenticate and communicate securely with the Kubernetes API server.
>
```yaml
--admission-control-config-file=/etc/kubernetes/policywebhook/admission_config.json
...
- mountPath: /etc/kubernetes/policywebhook
 name: k8s-admission
 readOnly: true
...
- hostPath:
   path: /etc/kubernetes/policywebhook
   type: DirectoryOrCreate
 name: k8s-admission
 ```

Contents of `admission_config.json`
>[!Note] "defaultAllow": false will stop pods to run if the webhook service is down

```json
{
   "apiVersion": "apiserver.config.k8s.io/v1",
   "kind": "AdmissionConfiguration",
   "plugins": [
      {
         "name": "ImagePolicyWebhook",
         "configuration": {
            "imagePolicy": {
               "kubeConfigFile": "/etc/kubernetes/policywebhook/kubeconf",
               "allowTTL": 100,
               "denyTTL": 50,
               "retryBackoff": 500,
               "defaultAllow": false
            }
         }
      }
   ]
}
```

`kubeconf` file
```yaml
apiVersion: v1
kind: Config

# clusters refers to the remote service.
clusters:
- cluster:
    certificate-authority: /etc/kubernetes/policywebhook/external-cert.pem  # CA for verifying the remote service.
    server: https://localhost:1234  # URL of remote service to query. Must use 'https'.
  name: image-checker

contexts:
- context:
    cluster: image-checker
    user: api-server
  name: image-checker
current-context: image-checker
preferences: {}

# users refers to the API server's webhook configuration.
users:
- name: api-server
  user:
    client-certificate: /etc/kubernetes/policywebhook/apiserver-client-cert.pem     # cert for the webhook admission controller to use
    client-key:  /etc/kubernetes/policywebhook/apiserver-client-key.pem             # key matching the cert
```
This is a similar mechanism to [[OPA Gatekeeper#whitelist trusted images]]