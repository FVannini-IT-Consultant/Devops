Refer to the man for examples
```bash
systemctl status ufw
ufw status

ufw status numbered

ufw enable

ufw reset

ufw allow 1000:2000/tcp

ufw allow 22/tcp

ufw allow from 135.22.65.0/24 to any port 9090 proto tcp
```