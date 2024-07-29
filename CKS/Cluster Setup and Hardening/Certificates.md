## Create the key and CSR
Create a key
```bash
openssl genrsa -out /root/60099.key 2048
```

Create a CSR for user `60099@internal.users`
```bash
openssl req -new -key /root/60099.key -out /root/60099.csr -subj "/CN=60099@internal.users"
```

## Manually sign the CSR
```bash
openssl x509 -req -in 60099.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out /root/60099.crt -days 5
00
```

## Sign using the API
https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#normal-user

`cat 60099.csr | base64 -w 0` or `cat 60099.csr | base64 | tr -d "\n"`

Then send to API

```bash
cat <<EOF | kubectl apply -f -
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: 60099@internal.users
spec:
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1pEQ0NBVXdDQVFBd0h6RWRNQnNHQTFVRUF3d1VOakF3T1RsQWFXNTBaWEp1WVd3dWRYTmxjbk13Z2dFaQpNQTBHQ1NxR1NJYjNEUUVCQVFVQUE0SUJEd0F3Z2dFS0FvSUJBUUMxTi9PWmlRTlNCeXJCdDFVVXk2bmppRms1CnBJZ2lLRFVPNTM1dWd6T1pZWFQ2VVpaV2c4bnN0U21MRDRFeDRKeHJqQXgxeExxV0Z0TjgrM1B2Q3JtR0xXcmQKRUZodkc5aU1xbjVRQ1RpNHJBaTFTdU9zN3BkRVRKMHUzSW1HTktmOWtsWGZLSDdrMEZ0NDFjK2FHTnB4dkJ5LwpmMEkwSnQ3SU1NLzloZlRCRmMrTEtkNHF0ZkJhc3RFZ2FkcTFsYnorK0J1VDFOVkdvTHAweXgzWEZUMUxJSWNFCld6dVU5QjBvdEpRay9WcGtGcUlrVFhla282UGZTTjFkUWJpNXQyUGFsNHJPM0p4WGdCV1VzT1V2R0xnbVV5d2sKNzd0UFJPZXFQcWZiL05aWGwyVWpPQXNnaHZPdW8zN2ZSUWw3UzBycWFSb1g1ZysxRTdwNlpzT0cySFBiQWdNQgpBQUdnQURBTkJna3Foa2lHOXcwQkFRc0ZBQU9DQVFFQVVZdjdNZlcwM0trall6S2JqdEJodDEwWDU2OWNMeXJnCkNIZ3NGVngyMDdVdjZUNFlHRm9uZmZSeW01TWUrYmsrdnQ0ZWQ4M0xuMHRjbkxNY0lOYUdkUXVjREdPaFhOaW8KelREZURDendJd1JTM25CSCtsZjc2SHlSZng4SHdwN0V4Uk94V2tOT3pGREVUTjY0bmtkclpYY2c2UHBCZjdJNApxODhPaFFLdVdjdHVTeGErQUhLQXZmZnJOaEFxdUVSa1dtdGQzS2xyTkZiUEtyMmc2Y2pvazcwSERHTWRsdDIyCllaNzlQZXJzWVg2TWhvcXVTQXJGZXQ5VmxoekdEdnFkQnY3Q2g4cTRibjRvcUZoY1ZGQVRXTFppVE80ZEZkQlIKdFNwVW0yQTYrekZwcEtzS0lrT2FLeVIrNCt2aFFFSG5qNkJzT1FzeUN4bW5vVS9HNUk1WDd3PT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
EOF
```

Verify the request is in pending state

```
k get certificatesigningrequests.certificates.k8s.io 
NAME                   AGE   SIGNERNAME                                    REQUESTOR                  REQUESTEDDURATION   CONDITION
60099@internal.users   7s    kubernetes.io/kube-apiserver-client           kubernetes-admin           24h                 Pending
```

Approve the request
```
k certificate approve 60099@internal.users

k get certificatesigningrequests.certificates.k8s.io 
NAME                   AGE     SIGNERNAME                                    REQUESTOR                  REQUESTEDDURATION   CONDITION
60099@internal.users   2m37s   kubernetes.io/kube-apiserver-client           kubernetes-admin           24h                 Approved,Issued
```

## Download crt file

This command retrieves the whole CSR
```bash
k get certificatesigningrequests.certificates.k8s.io/60099@internal.users -o yaml
```
Whereas this retrieves a crt

```bash
k get certificatesigningrequests.certificates.k8s.io/60099@internal.users -o jsonpath='{.status.certificate}' | base64 -d > 60099.crt
```
