
## Autocompletion
https://kubernetes.io/docs/reference/kubectl/quick-reference/#kubectl-autocomplete
### Bash
This requires [[Bash completion]]
```bash
source <(kubectl completion bash) # set up autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```

```bash
alias k=kubectl
complete -o default -F __start_kubectl k
```
