- Generate SSH Key Pair (first more modern)
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

- ssh into `node01` host from `controlplane` host

`ssh node01`

- Create user `jim` on `node01` host

`adduser jim`

- Back to `controlplane` host and copy ssh public key

`ssh-copy-id -i ~/.ssh/id_rsa.pub jim@node01`

- Test ssh access from `controlplane` host

`ssh jim@node01`

### Harden SSH Daemon

Disable root login and password authentication
in `/etc/ssh/sshd_config`

```
...
PasswordAuthentication no
PermitRootLogin no
...
```

`service sshd restart`
## Sudoers

in `/etc/sudoers` (remember to chmod 644 and back to 440)

`jim   ALL=(ALL:ALL) ALL` Will allow the user to use sudo
`jim ALL=(ALL) NOPASSWD:ALL` Allow the user to not need sudo command

Groups are indicated with a %
`%admin ALL=(ALL) ALL`