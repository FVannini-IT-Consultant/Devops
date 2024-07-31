Find process

`netstat -tulnap | grep 1234` If not present `apt install net-tools`
or
`lsof -i :1234`

Find its path
`ls -l /proc/<ID>/exe` This returns the path of the file referenced by this symbolic link e.g. `/usr/bin/app1`

Kill process
`kill <ID>` or use `kill -9 <ID>`

Remove the file
`rm /usr/bin/app1`

