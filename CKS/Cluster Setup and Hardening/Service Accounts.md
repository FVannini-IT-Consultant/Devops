- Managed by the API
- Each namespace has a default one

 `k get sa sa-tink -n tinkering -o yaml`

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: "2024-09-02T07:48:33Z"
  name: sa-tink
  namespace: tinkering
  resourceVersion: "230347"
  uid: 90ef2bc8-c28f-48b8-9556-1f9b616d786b
```

## Token

Create a JSON Web Token (JWT)
`k create token sa-tink -n tinkering` 

`eyJhbGciOiJSUzI1NiIsImtpZCI6Ik1HNnJ0R1NEb1I1UTZOWV91Z21MZkY0YW1mbHlyR1NISURjdVFNZldjS2cifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzI1MjY3MTA4LCJpYXQiOjE3MjUyNjM1MDgsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwianRpIjoiNGRkMjMyMDYtM2ZmYS00ZmQ0LThmODItNDViYzk0ZTNlYTcwIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJ0aW5rZXJpbmciLCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoic2EtdGluayIsInVpZCI6IjkwZWYyYmM4LWMyOGYtNDhiOC05NTU2LTFmOWI2MTZkNzg2YiJ9fSwibmJmIjoxNzI1MjYzNTA4LCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6dGlua2VyaW5nOnNhLXRpbmsifQ.T9oYi_KDzREjCoANouUcBsDx3xk2qsY5QCmRMonNpEJlkqUVbp5znQU8eiqJ5pMuCJjaAcmOgbGH3Ml6jLvF2s2MdRqbIntLq0Efr_k6l2YxCvc0es8w4VMXJ9uuhi3ZNPAAmHZvtIOvBGflEf4rFvk4X_RneY83qrjdV1GInyvXAm42vYT0j2HG9mHJ5O_Mtuc6sT8wANnvPulLEKf60ZXF2Q8Geuj1r7cMh41bCr7_7Qf8wtIGQhKARsK0P9JHPIX78_3Vo7jA4nLW9EFQaY5fTrtQd5RN9i74BcHs0DFVQ34ta02aBeetJ0GQFoGkB9HVMXbYN90HGMzrTnlZ2Q`

Decode it in [[https://jwt.io/]] 

Create a pod that uses the SA
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod-tink
  name: pod-tink
  namespace: tinkering
spec:
  serviceAccountName: sa-tink
  containers:
  - image: nginx
    name: pod-tink
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
`k exec -n tinkering pod-tink -it -- sh`

```bash
cd  /run/secrets/kubernetes.io/serviceaccount
# ls
ca.crt  namespace  token
```
Where token is a newly created token from sa-tink just for the pod.

## Contact the API from the Pod

```bash
env
...
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_SERVICE_HOST=10.96.0.1
...
```
This way we are system anonymous
```bash
curl https://10.96.0.1 -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```
But in this way we are authenticated

```bash
curl https://10.96.0.1 -k -H "Authorization: Bearer $(cat token)"
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:serviceaccount:tinkering:sa-tink\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```
This is **often not needed and dangerous** so we need the SA to not mount the Token automatically using `automountServiceAccountToken: false` either in the SA or in the POD
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#opt-out-of-api-credential-automounting


