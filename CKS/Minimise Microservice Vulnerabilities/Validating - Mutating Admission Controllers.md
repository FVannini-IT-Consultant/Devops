
`k create ns webhook-demo`

```bash
k create secret tls webhook-server-tls --cert=/root/keys/webhook-server-tls.crt --key=/root/keys/webhook-server-tls.key -n webhook-demo 
secret/webhook-server-tls created
```

Example Mutating Webhook Configuration - POD Create

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: demo-webhook
webhooks:
  - name: webhook-server.webhook-demo.svc
    clientConfig:
      service:
        name: webhook-server
        namespace: webhook-demo
        path: "/mutate"
      caBundle: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURQekNDQWllZ0F3SUJBZ0lVRklFMVZFZmZTcXFnRUxZSzhsR2RZQkh4eEJNd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0x6RXRNQ3NHQTFVRUF3d2tRV1J0YVhOemFXOXVJRU52Ym5SeWIyeHNaWElnVjJWaWFHOXZheUJFWlcxdgpJRU5CTUI0WERUSTBNRGN3TlRFeE5EVXlPVm9YRFRJME1EZ3dOREV4TkRVeU9Wb3dMekV0TUNzR0ExVUVBd3drClFXUnRhWE56YVc5dUlFTnZiblJ5YjJ4c1pYSWdWMlZpYUc5dmF5QkVaVzF2SUVOQk1JSUJJakFOQmdrcWhraUcKOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQWx5RURJcFhQVTI2VmhzQ2x2VE9mY3hRZk1aZU5kbnduOWxJeQpaVVlyYzJVK0VjcGE5VUVOSnZwRVJFaW9adUluNWhFVDV3YzNtOURtNVdJZ0tEMnBReHgrNzh4bW05NWJPR3lqCnJmOWNSbUErQzZzQ1BReFF5Q1hLZDg4cm5SUDA3aldvRHVFaXJybkdYREI3Nm15MS9DOWRMazJ2bExxRlFtMHkKbkZyMXJkL24wT1ZrVU9LVkdteGJXSDFlMmlWVHFKZ0dWTGhsaWljOXVUMXRFSG9tNkZDRjFuWVRhZHJaQzA2cgpPMmNGSHdXcE5iU29WZGtkZGtHMWZyYnFuRU1XbldyZStkdVhCUkFFWkVCbTVXMCtwZnBmcVo1eXFpc3VWcWdOCm15SXh0S2Q0SitOTGMwNTdhY290Rnc5K2l6anlMdXhkbHd2UTBhVmdTOVM5bmFrQ1hRSURBUUFCbzFNd1VUQWQKQmdOVkhRNEVGZ1FVNDI4M1EvQlpURjQ3MjRUbVR4R3ZORXNCcnVNd0h3WURWUjBqQkJnd0ZvQVU0MjgzUS9CWgpURjQ3MjRUbVR4R3ZORXNCcnVNd0R3WURWUjBUQVFIL0JBVXdBd0VCL3pBTkJna3Foa2lHOXcwQkFRc0ZBQU9DCkFRRUFIdHAraHozdWUvdlhHY2Y5bmU2azZLN3VicmxZcys1dDg5dnlOUk0vdUlOYThtaDV4TW9XclVXVWFSZGMKM0JGb2V6dldjemVrR3ZHdnBEMlN5cmtYV1dwSk0rQ3dZM2VIa3pQUkFxZTdYS2h1QkxTYmN5WFFiR2lLZDRpZwo3Q2dQMGxFTTZWZXRYSzNIbElQeHNHZGU0UHFwdnAyaGcvUXFTQmpkaERSMEdIekJGelY0YmxjSlZzaFJIQkZYClZIeWJxelVabjhWamhVTFNoUlU0WXZxV3Exc01nRWlSc0VxUVk5K1lCc2NYbkJjbEpsSUljVmpJcE1PMS9lTm4KM1h0ZUpnUkx6WWUxbC8zaFRCUld4Z04vdXpVdHViUEJZYlZkSDk4S0lsOSt5TWVUajNHNjdWblZ6WGdnYkN5eApnRG1aVHhkZU80Uyt5TThUUUxTbFBWeFQ2dz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
    rules:
      - operations: [ "CREATE" ]
        apiGroups: [""]
        apiVersions: ["v1"]
        resources: ["pods"]
    admissionReviewVersions: ["v1beta1"]
    sideEffects: None
```

**A deployment and service need to be created for the webhook server** 

Pods that will run

```yaml
securityContext:
   runAsNonRoot: false
```
Or no security context at all

Pods that will not run

```yaml
securityContext:
    runAsNonRoot: true
    runAsUser: 0
```