Open Policy Agent

Installation:
```bash
curl -L -o opa https://github.com/open-policy-agent/opa/releases/download/v0.11.0/opa_linux_amd64
chmod 755 ./opa
./opa run -s
```

Example policy
```rego
package httpapi.authz

# HTTP API request
import input
default allow = false

allow {
  input.path == "home"
  input.user == "john"
}
```

Load the policy
```bash
curl -X PUT --data-binary @example.rego http://localhost:8181/v1/policies/example1
```

View current policies
```bash
curl http://localhost:8181/v1/policies
```

Kubernetes uses [[OPA Gatekeeper]]

Resources
- Rego language https://www.openpolicyagent.org/docs/latest/policy-language/
- Playground https://play.openpolicyagent.org/
- Policy testing https://www.openpolicyagent.org/docs/latest/policy-testing/
- Killer Shell https://www.youtube.com/watch?v=d9xfB5qaOfg