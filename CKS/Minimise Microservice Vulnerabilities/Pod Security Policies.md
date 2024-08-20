#admissioncontroller #labels 
PSA & PSS Pod Security Admission and Standards (Once PSP Pod Security Policy)

Enabled via default
```
kubectl exec -n kube-system kube-apiserver-controlplane -it -- kube-apiserver -h | grep enable-admission-plugins
```
PSS Profiles: https://kubernetes.io/docs/concepts/security/pod-security-standards/
- Privileged: Unrestricted policy
- Baseline: Minimal restrictive policy
- Restricted: Heavily restricted policy
PSA Mode: https://kubernetes.io/docs/concepts/security/pod-security-admission/
- enforce: Reject pod
- audit: Record in the audit logs
- warm: Trigger user-facing warning

PSS/PSA applied at the namespaces level
```bash
kubectl label ns payroll pod-security.kubernetes.io/enforce=restricted

kubectl label ns beta pod-security.kubernetes.io/enforce=baseline pod-security.kubernetes.io/warn=restricted
```
Example admission configuration
```yaml
--- admission-configuration.yaml 
apiVersion: admissionregistration.k8s.io/v1
kind: AdmissionConfiguration
plugins:
  - name: PodSecurity
    configuration:
      apiVersion: pod-security.admission.config.k8s.io/v1
      kind: PodSecurityConfiguration
      defaults:
        enforce: baseline
        enforce-version: latest
        audit: restricted
        audit-version: latest
        warn: restricted
        warn-version: latest
      exemptions:
        usernames: [] 
        runtimeClassNames: [] 
        namespaces: [my-namespace] 
```
A way to regulate privileged, runAsUser, CAP_SYS_BOOT, CAP_SYS_TIME, etc.

- Course material https://learn.kodekloud.com/user/courses/certified-kubernetes-security-specialist-cks/module/7431dd03-f5c2-4ebb-b94a-2d35615bbd8c/lesson/a3ca9b49-2589-4497-afb1-213b9c4dc47d