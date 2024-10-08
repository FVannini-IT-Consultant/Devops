#pod #admissioncontroller
OPA Gatekeeper is an implementation of [[OPA]]
Policy engine for enforcing security policies.
_Example: Ensures that all deployed containers must have specific security labels_
**Needs**: OPA policies written in Rego, deployed as a Kubernetes admission controller.
Provides K8s CRDs Custom Resource Definitions

How to use Gatekeeper https://open-policy-agent.github.io/gatekeeper/website/docs/howto/

It is configured to work with validating [[Admission webhooks]]

Install
https://open-policy-agent.github.io/gatekeeper/website/docs/install
```bash
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/v3.16.3/deploy/gatekeeper.yaml
```

Provides Custom Resource Definitions (CRD)

What - Where - How (Constrain template)
- What -All obj have specific label
- Where -Enforced on admission controller - `target: admission.k8s.gatekeeper.sh`
- How -Get labels of obj and check if has the right one - With a rego policy

From a Constraint Template you then implement Constraints

Constraint Template 
```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
```
-> Constraint
```yaml
apiVersion: constraint.gatekeeper.sh/v1beta1
kind: k8srequiredlabels
metadata:
  name: pod-must-gave-gk
```
-> Constraint
```yaml
apiVersion: constraint.gatekeeper.sh/v1beta1
kind: k8srequiredlabels
metadata:
  name: ns-must-gave-gk
```

#### Whitelist trusted images

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8strustedimages
spec:
  crd:
    spec:
      names:
        kind: K8sTrustedImages
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8strustedimages

        violation[{"msg": msg}] {
          image := input.review.object.spec.containers[_].image
          not startswith(image, "docker.io/")
          not startswith(image, "k8s.gcr.io/")
          msg := "not trusted image!"
        }
```

```yaml
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sTrustedImages
metadata:
  name: pod-trusted-images
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
```

Resources
OPA Gatekeeper Library https://open-policy-agent.github.io/gatekeeper-library/website/
OPA Gatekeeper Demo Github https://github.com/open-policy-agent/gatekeeper/tree/master/demo
OPA Gatekeeper Resources https://github.com/open-policy-agent/gatekeeper/tree/master/example/resources
K8S reference https://kubernetes.io/blog/2019/08/06/opa-gatekeeper-policy-and-governance-for-kubernetes/