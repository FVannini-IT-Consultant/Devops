#container #pod 
Runtime security tool for detecting anomalous behavior.
_Example: Alerts you if a container suddenly starts running unexpected processes._
**Needs**: Installation of Falco as a DaemonSet, rules configuration in YAML.

strace on steroids
https://falco.org/docs/getting-started/falco-kubernetes-quickstart/


It typically works on a per node basis as a service or directly from terminal
`systemctl status falco`

It logs in `/var/log/syslog`

Default rules in `/etc/falco/`
```
falco.yaml
falco_rules.local.yaml
falco_rules.yaml
k8s_audit_rules.yaml
...
```
Change a rule to a custom
`grep -r "Some text from an exisiting rule" /etc/falco`

To add fields to the output
https://falco.org/docs/reference/rules/supported-fields/

Enable file output in `etc/falco/falco.yaml`
```yaml
file_output:
  enabled: true
  keep_alive: false
  filename: /opt/security_incidents/alerts.log
```
### Reload falco

- Hot reload `kill -1 $(cat /var/run/falco.pid)`
- Restart `systemctl restart falco`