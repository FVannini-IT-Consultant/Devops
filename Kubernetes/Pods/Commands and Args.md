
Declarative

```yaml
spec:
  containers:
  - name: ubuntu
    image: ubuntu
	command: [ "sleep", "5000" ]
```

```yaml
spec:
  containers:
  - name: ubuntu
    image: ubuntu	
	command:
	  - "sleep"
	  - "5000"
```

```yaml
spec:
  containers:
  - name: ubuntu
    image: ubuntu
	command: [ "sleep" ]
	args: [ "5000" ]
```

```yaml
spec:
  containers:
  - name: ubuntu
    image: ubuntu
    command:
    - sh
    - -c
    - sleep 5000
```

Imperative

`k run ubuntu --image=ubuntu --command -- sleep 5000`

`k run ubuntu --image=ubuntu --command -- sh -c 'sleep 5000'`
